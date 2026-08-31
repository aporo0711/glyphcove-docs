# アーキテクチャ

**English**: [architecture.md](architecture.md)

GlyphCove は、VS Code 拡張機能が持つ 2 つのプロセスの中で、すべてお使いのマシン上で動きます。

- **拡張機能ホスト**は、ファイルとディスクに触れる処理のすべてを持ちます。文書を開くこと、
  エディタから返ってきた編集を適用すること、そして各種変換（HTML / PDF / Word / Excel と
  その逆方向）の実行です。図（Mermaid / PlantUML）と数式（KaTeX）の描画もローカルで行い、
  文書の内容がマシンの外へ出ることはありません。
- **Webview エディタ**は、編集するプレビューを描画します。編集したときに再生成されるのは
  実際に触れたブロックだけなので、編集していない Markdown はバイト単位で不変のまま残ります
  （本製品の第一の設計原則です）。

両者の境界を越えるメッセージはすべて、両側で受信時に検証されます。Webview は厳格な
Content-Security-Policy の下で動きます（意図して設けた唯一の例外を、最初の ADR に
記録しています）。プレビューと変換結果は同じ描画コードを共有しているので、
エディタに見えているものがそのまま変換の出力になります。

## ADR（設計判断の記録）

- **ADR: Mermaid 描画のため `style-src` にのみ `'unsafe-inline'` を追加**（v0.1.2〜）:
  mermaid はラベル計測と出力 SVG の双方でインライン `<style>` を注入し、厳格な style-src
  下では動作しない。代替案（SVG から style を抽出して自前スタイルシートへ移す）は計測時の
  フォント不一致でレイアウトが崩れるため不採用。**script-src は nonce のみのまま**で、
  `img-src`/`font-src`/`connect-src` も不変。mermaid は `securityLevel: 'strict'` +
  `htmlLabels: false` で実行し、出力 SVG は **DOMPurify（SVG プロファイル）を通してから**
  DOM に挿入する。VS Code 標準の Markdown プレビューが style-src `'unsafe-inline'` を
  許可しているのと同じ妥協点
- **ADR: PlantUML の描画エンジンは「jebbs.plantuml 拡張同梱の plantuml.jar の再利用」を採用**（v0.1.2〜）:
  PlantUML には Webview 内で動くオフライン JS 版が存在しないため、描画はホスト側の
  Java プロセス（`java -jar plantuml.jar -tsvg -pipe`、引数配列で spawn・シェル不使用）。
  jar の入手は 3 案 —（a）MIT エディション（v1.2026.6 で約 17.6MB）を拡張に同梱、
  （b）PlantUML 拡張機能（jebbs.plantuml）が同梱する jar の再利用、（c）設定でユーザー指定 —
  を比較し、**（b）を基本 +（c）`glyphcove.plantuml.jarPath` で上書き可**とした。
  jebbs.plantuml v2.18.1 が拡張ルート直下に `plantuml.jar`（実測 11,863,156 バイト ≒ 11.9MB）を同梱していることを
  実機で確認済み（draw.io 拡張同梱アプリの再利用と同じ前例）。（a）は VSIX が
  約 +18MB 肥大するため不採用（(b) が使えない環境は (c) で補える）。
  **jar のライセンス版は利用者が選ぶ**（同梱しない以上、選ぶのは利用者側になる）。
  PlantUML は同一の本体を GPLv3 / GPLv2 / LGPL / ASL / BSD / EPL / MIT で配布しており
  （<https://plantuml.com/en/download>、2026-08-17 参照）、MIT 版は公式リリースに
  `plantuml-mit-<version>.jar` として置かれている。公式の対応表で MIT 版に印が無いのは
  Ditaa / Jcckit / Sudoku / ELK の 4 つで、UML 図の生成能力は欠けない。
  **広告表示は MIT 版でも消えない** — 「welcome / エラー画像にスポンサーまたは広告の
  メッセージを表示することがある」という一文は、GPL 版・MIT 版の双方の `-license` 出力に
  同じ文言で入っている。本製品は構文エラーのときはエラー画像を捨てるので通常経路では
  表に出ないが、**「MIT 版にすれば広告が消える」とは書けない**。手元の jar の版は
  `java -jar <jar> -license` で判別できる（公式 FAQ が案内している方法）。
  設定でパスを明示した場合は**そのパスが見つからなければエラー**（自動検出へ黙って
  フォールバックしない）。Java / jar が無い環境では描画せず案内を表示するだけ（正直な劣化）
- **ADR: 数式は KaTeX 単独採用。MathJax は採らない**（v0.1.2〜）:
  描画エンジンの候補は（a）KaTeX、（b）MathJax、（c）両対応の切り替え設定。
  **（a）を採用**した。理由は 3 つ —
  ① **同期 API で描画できる**（`katex.renderToString`）。プレビューの NodeView も
  HTML/PDF 変換も**同じ 1 関数**（`shared/math.ts` の `renderMath`）を呼べるので、
  「プレビューと変換結果が食い違わない」という本製品の基本方針を**構造的に**満たせる。
  MathJax v3 の描画はブラウザ DOM を前提とするため、ホスト側（Node）で使うには
  Mermaid と同じ**ヘッドレスブラウザ + ループバックサーバ**が必要になり、
  PDF 変換に外部ブラウザ依存を 1 つ増やす。
  ② **容量が小さい**（katex.min.js 約 270KB + woff2 約 260KB。mermaid の 3.5MB に対して
  誤差の範囲）ので、Mermaid のような遅延ロード機構も CSP の追加も要らない。
  ③ 本製品が扱うのは業務文書であり、MathJax が優位に立つ領域
  （`\require`、独自マクロパッケージ、AsciiMath 等）を必要としない。
  （c）は「設定で描画結果が変わる = 文書の見え方が環境依存になる」ため不採用
  （必要になったら ADR を書き直す）
- **ADR: インライン数式は `$$…$$`。単独の `$…$` は数式として扱わない**（v0.1.2〜）:
  remark-math の既定（`singleDollarTextMath: true`）を実測したところ、**既存文書を壊す**
  ことが分かったため無効化した。実測結果:
  - パーサ側: `価格は $100〜$200 です。` が `inlineMath("100〜")` に、
    `環境変数 $HOME と $PATH` が `inlineMath("HOME と ")` になる
    （同じ行に `$` が 2 つあれば成立してしまう）
  - **シリアライザ側**: `mdast-util-math` が `$` を要エスケープ文字として登録するため、
    通貨に触れているだけのブロックを 1 文字編集すると `$100` が **`\$100` に書き換わる**
    （GitHub Alerts のマーカーが `\[!NOTE]` になったのと同型の事故）

  `singleDollarTextMath: false` では単独の `$` は両側で完全に不活性になり、
  通貨表記はバイト単位で不変のまま往復する。エスケープされるのは `$` が
  2 個以上連続した場合だけ（`$$` → `\$$`）で、これは曖昧さを避けるために必要な変換。
  **この値は `shared/math.ts` の `MATH_OPTIONS` 1 箇所で持ち、パーサとシリアライザの
  両方へ同じものを渡す**（片方だけ変えると往復が壊れる）。
  代償として GitHub の `$…$` 記法とは非互換になるため、[既知の正規化・制限](guide/limitations.ja.md) に明記した
