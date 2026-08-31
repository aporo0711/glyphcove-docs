# セキュリティ

**English**: [security.md](security.md)

- Webview CSP: `default-src 'none'` + nonce付きスクリプトのみ。**script-src に `unsafe-inline` / `unsafe-eval` は不使用**
- Mermaid 本体（mermaid.min.js 3.5MB）は拡張に同梱し、**必要になるまで読み込まない**
  （最初の Mermaid ブロックで nonce 付き `<script>` を注入。CDN 不使用・完全オフライン）
- Markdown由来HTML: エディタは micromark＋DOMPurify、HTML変換は rehype-raw＋rehype-sanitize で無害化（script/イベント属性/`javascript:` はテストで除去確認）
- Webview⇔Host間の全メッセージを型・長さ・値域でスキーマ検証（両方向）
- 外部プロセスは実行ファイル＋引数配列でspawn（シェル文字列連結なし）。数式注入対策（Excel出力は`=`等開始セルを文字列化）
- `localResourceRoots`（Webviewが読めるローカル資源）は仕様§7.1の「必要最小限」を維持:
  拡張機能の `dist/webview` に加え、**画像表示のために「開いている文書のフォルダ」と
  「その文書を含むワークスペースフォルダ」のみ**を許可（v0.1.2〜。ホームフォルダ等は含めない）。
  CSP `img-src` に `http(s)` は含めず、外部画像は読み込まない
- ステンシルの自動描画（Draw.io Integration 拡張 + Edge/Chrome がある場合のみ）:
  同拡張が同梱する draw.io 本体を、**ヘッドレスブラウザ + ループバック HTTP サーバ**で
  ホスト側に隔離して描画に使う（PDF 変換と同じくブラウザは同梱せず、インストール済みの
  ものを引数配列で起動）。draw.io が要求する `unsafe-inline`/`unsafe-eval` は
  そのループバックページ内で完結し、**エディタ Webview の CSP・localResourceRoots は
  一切緩めない**。サーバは 127.0.0.1 の一時ポートに**ランダムトークン付きパス**で bind し、
  配信するのは draw.io の webapp（読み取りのみ）と駆動用ページ 2 枚だけ
  （文書の内容は一切配信しない）。描画結果でのファイル上書きは、
  **このセッションで貼り付けが作ったプレースホルダに限り**（ホストが URL を追跡し、
  ファイル内容のマーカーも再確認してから）行う
- Mermaid の SVG 事前変換（HTML/PDF 変換時）も同じ隔離方式:
  ループバック HTTP サーバ（127.0.0.1・ランダムトークン付きパス）が配信するのは
  同梱の mermaid.min.js / purify.min.js と生成ページ・図のソースだけで、
  ヘッドレスブラウザは 1 バッチごとに使い捨て。ページ内 CSP はインラインスクリプト・
  eval・外部通信を禁止し、SVG は**ページ内で DOMPurify を通してから**回収・検証する
- KaTeX は **`trust: false`**（既定）で実行する: `\href` / `\url` / `\includegraphics` は
  解釈されず、数式から生の HTML やリンク・外部参照は生成されない（単体テストで固定）。
  KaTeX が出力する文字列はすべてエスケープ済みというのが KaTeX の契約だが、
  数式は文書由来の入力なので、**プレビューへ入るものはさらに DOMPurify を通す**
  （mermaid / PlantUML と同じ最終ゲート）。`maxSize` / `maxExpand` でマクロ展開と
  サイズ指定の暴走も抑えている。**CSP は変更なし**:
  KaTeX はインラインスタイルを出すが `style-src` は Mermaid 対応で既に `'unsafe-inline'`、
  フォントの `font-src` は既に `cspSource`、`script-src` は nonce のみのまま
- PlantUML プロセスは **`PLANTUML_SECURITY_PROFILE=SANDBOX`** で起動する:
  既定プロファイルでは `!include C:/…` で**ローカルファイルの内容が図に取り込めてしまう**
  ことを実機で確認したため、ファイル・ネットワークアクセスを遮断する SANDBOX を必須とした
  （jar 内蔵の標準ライブラリ `!include <C4/…>` 等は SANDBOX でも動作することを確認済み。
  `javascript:` ハイパーリンクは PlantUML 自身が除去することも確認）。加えて出力 SVG は
  ホスト側で形状検証（`<script>`・`javascript:` href の拒否）し、プレビューへ入るものは
  さらに Webview 側で **DOMPurify（SVG プロファイル）**を通す（mermaid と同じ最終ゲート）。
  常駐プロセスは stdin/stdout のパイプだけで通信し、**ネットワークを一切開かない**。
  構文エラーは PlantUML が「エラー画像」を出力するが、**stderr の報告（-stdrpt）で検出して
  エラー画像は捨て**、行番号付きメッセージとして表示する（エラーを成功として埋め込まない）。
  ⚠ **`-stdrpt` は構文エラーしか報告しない** — PlantUML 自身が異常終了した場合は
  報告が出ないまま、エラー画像が終了コード 0 で返る（実測）。そのため
  **stderr の Java スタックトレースも失敗の証拠として見る**。エラー画像には
  PlantUML 側の誘導文言と**図のソースを埋め込んだ QR コード**が入るため、
  これを成功として埋め込むと文書の中身が画像として外に出る。
  ⚠ 例外の**メッセージは記録しない**（文書の中身を引用しうるため）。クラス名だけを扱う
- **図の SVG を PNG にするページは、その SVG を `<img>` として読み込む**（ページの DOM には入れない）:
  文書に書いた `.svg` ファイルはサニタイザを通さずに読むため（`foreignObject` のラベルを
  落とさないため）、**ページに直接埋めると文書がページの DOM を持つ**ことになる。
  ⚠ **`default-src 'none'` だけでは足りない** — CSP には**遷移を止める指示が無い**ので、
  `foreignObject` の中の `<meta http-equiv="refresh">` は素通りして外部へ接続する（実測）。
  `<img>` に入れた SVG はスクリプトも外部参照も遷移も持たない（同じ条件で実測して遮断を確認）。
  **「完全オフライン・送信ゼロ」を守っているのはこの読み込みかたであって、CSP ではない**
