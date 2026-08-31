# GlyphCove for VS Code

Markdown を見たまま編集し、Word・Excel・PDF・HTML と相互変換するエディタ拡張。

> **本拡張機能の利用には [GlyphCove 使用許諾契約書](../LICENSE)（版 1.2）が適用されます。**
> インストール・複製または使用により、本契約に同意したものとみなされます。
> 全文は同梱の [LICENSE](../LICENSE) にあり、要点は[ライセンス](#ライセンス)節にまとめています。

**English**: [README](../README.md)

変更履歴: [CHANGELOG.md](../CHANGELOG.md) ／ フィードバック: [CONTRIBUTING.md](../CONTRIBUTING.md)

![Excel で選択した範囲を GlyphCove のプレビューへ貼り付けているところ](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/gifa-ja.gif)

*Excel でコピーした表を貼ると、Markdown の表になります。*

## こういう方のための拡張機能です

設計書・議事録・報告書を Markdown で書きたいのに、提出先が Word や Excel や PDF、という方のためのものです。

**Markdown で書いて、Word で納品する。**

- **Word をやめる必要はありませんし、既存の資産を作り直す必要もありません。** 変わるのは書いている途中だけで、提出はこれまでどおりです。**新しく書くものから**で十分です
- **ファイルはあなたのものです。** 編集していないブロックは 1 バイトも書き換えません — 選んだフェンス記号も、手で折り返した行もそのままです。Git の差分には直したところだけが出ます。貼り付けた画像は文書の隣にファイルとして置き、base64 は作りません
- **文書は端末から出ません。** クラウドの API もテレメトリもページ課金もありません。プレビューと Word / Excel 変換は外部の `http(s)` 画像も読み込みません。はっきり言うと、**作者にも、あなたが何を変換したかは分かりません**（知る手段がありません）
- **変換で失われたものは、隠さず報告します。** Markdown が Word のすべてを表現できないのは当然で、逆もまた同じです。落ちたもの・平坦になったものは警告と品質レポートで申し上げます。**列が 1 つ黙って消えたファイルは、変換の代償ではなく不具合として扱います**
- **記法を覚えなくても、文書は受け取れます。** Excel・Word・Web ページ・罫線で描かれた端末出力から貼った表は表のまま入り、見出し・箇条書き・図はプレビュー画面のまま編集できます

Markdown エディタそのものをお探しなら、VS Code にはすでに良いものがあります。本拡張機能が引き受けるのはその先 — **相手が指定した形で文書を出すところ**です。

## できること

- **プレビュー画面のまま編集できます。** 記法を意識せずに書け、**編集していないブロックは 1 バイトも書き換えません**（Git の差分は直したところだけになります）
- **図と数式がその場で描けます。** Mermaid・PlantUML・KaTeX の図と数式をプレビュー上で描画し、クリックするとソースが開いてライブ更新されます。draw.io の図はダブルクリックで編集できます
- **表を貼り付けられます。** Excel・Word・Web ページ・罫線で囲まれたテキスト・CSV / TSV から、表のまま入ります
- **画像は外部ファイルとして持ちます。** 貼り付けた画像は `<文書名>_images/` へ保存し、base64 は作りません
- **Word / PowerPoint へ書式のまま貼れます。** ツールバーの「Office」ボタンでコピーします
- **壊れたリンクを「問題」パネルに出します。** 存在しない相対リンク・画像・`#見出し` アンカーを、開いている文書について検査します

![GlyphCove のプレビューで Markdown を編集しているところ](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/sp1-ja.png)

*プレビューで Markdown を編集しているところ。*

![端末に出た罫線の表を GlyphCove のプレビューへ貼り付けているところ](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/gifb-ja.gif)

*端末に出た罫線の表も、貼れば表になります。*

## できないこと

- **外部 `http(s)` 画像は、プレビューと Word / Excel 変換では読み込みません** — 文書を開いただけで通信が起きない、という設計のためです。既知の例外が 1 つ: PDF 変換はローカルのブラウザに印刷させる方式のため、文書に外部画像が含まれているとそのブラウザが取得します（[詳細と回避策](guide/limitations.ja.md)。抑止は対応を予定しています）
- **スキャン PDF の OCR、PDF のヘッダー/フッターの抽出はできません**（黙って飛ばさず、警告で明示します）
- **PDF の表は貼り付けでは取り込めません** — クリップボードに表の構造が載らないためです。描画命令を読む PDF → Markdown 変換をお使いください
- **Excel から貼った結合セルは列がずれます**（既知の制限 — Excel の表は無変換で貼るためです。Word・Web ページの表は貼り付け時に結合を解除し、通知します）
- **数式は `$$…$$` だけです。** Word 出力では数式は描画されず、TeX ソースの等幅テキストになります
- **共同編集・クラウド同期・モバイル版はありません**

理由込みの全リストは[既知の正規化・制限](guide/limitations.ja.md)と[やらないことリスト](guide/not-planned.ja.md)にあります。

## 変換できる組み合わせ

![形式どうしの変換の対応を示した図](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/s3-matrix-ja.png)

*何から何へ変換できるか。*

| 変換 | ロス |
| --- | --- |
| Markdown → HTML | なし |
| Markdown → PDF | なし |
| Markdown → Word（DOCX） | あり |
| Markdown の表 → Excel（XLSX） | あり（表以外は別のシートへ） |
| PDF → Markdown | あり |
| Word（DOCX）→ Markdown | あり |
| Excel（XLSX）→ Markdown | あり |
| HTML → Markdown | あり |
| Word（DOCX）→ HTML | あり |
| Excel（XLSX）→ HTML | あり |

複数の `.md` を 1 つの PDF / HTML / Word / Excel にまとめることもできます。**ロスのある変換は、何が失われたかを警告で正直に報告します。逆変換(PDF / Word / Excel / HTML → Markdown)では品質レポートも付きます。**

![Markdown から変換した Word ファイルを Word で開いているところ](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/gifc-ja.gif)

*Markdown を Word へ変換したところ。目次は本物のフィールドで、Ctrl+クリックで見出しへ飛びます。*
**変換した `.docx` を Word で開いて見せています。**

![変換で運びきれなかったものを一覧にした品質レポート](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/s2-report-ja.png)

*変換で運びきれなかったものを、品質レポートが名指しします。*

## 表示言語と出力の言語

- **画面（UI）は VS Code の表示言語に従います** — 日本語と英語に対応しています
- **変換結果に差し込まれる文言は、実行者の表示言語ではなく「その文書の言語」に従います。** 英語の Markdown を Word にすれば目次は `Contents`、GitHub Alerts の囲みは `Note` / `Tip` / `Important` / `Warning` / `Caution`、図表番号は `Figure 1` / `Table 1`、透かしは `CONFIDENTIAL` になります。日本語の文書なら「目次」「ノート」「図 1」「社外秘」です。生成する HTML はそれに合った `lang` を宣言します
- 言語は文書から読みます — front matter の `lang:`（`language:` / `言語:` も可）があればそれ、無ければ本文の文字種から判定します。**どちらも入力だけで決まるので、同じ入力からは常に同じファイルができます**（変換を実行した人によって成果物が変わりません）。言語の手がかりが 1 つも無い文書（数値だけの表など）のときだけ、表示言語に倒します
- **変換について報告する文言は、実行者の表示言語に従います** — 変換ログ（`.log.txt`）・品質レポート・警告の文。これらは文書ではなく「その実行」を説明するものです

## 完全オフライン

- **クラウド API への送信は一切ありません。** PDF 印刷もローカルのブラウザ実行ファイルを使います
- **外部 `http(s)` 画像は、プレビューと Word / Excel 変換では読み込みません**（文書を開いただけで外部通信が起きない設計です。既知の例外は PDF 変換 — [詳細](guide/limitations.ja.md)）
- 未信頼ワークスペースでは、外部プロセスを起こす動作を一切行いません（変換も、プレビューや「Office 用にコピー」の図の描画もしません。Workspace Trust）

## 動作条件

| 要件 | バージョン | 用途 |
| --- | --- | --- |
| VS Code | 1.90 以上 | 本体 |
| Microsoft Edge または Google Chrome | インストール済みのもの | PDF 変換、図の SVG 事前変換と PNG 化（自動検出） |
| Java + plantuml.jar（任意） | Java 8 以上 | PlantUML 図の描画。**jar は同梱していません**（どの版を使うかは利用者が選べます。MIT 版も可 — [配布ページ](https://plantuml.com/ja/download)。PlantUML 拡張機能（`jebbs.plantuml`）が入っていれば同梱の jar を自動で再利用するので、入手は不要です）。無ければ案内を出し、変換はコードブロックのまま出力します |

## 使い始める

- `.md` ファイルを右クリック →「**プレビュー編集で開く**」でプレビュー編集
- ファイルを右クリック →「**ファイルを変換 (Markdown / PDF / Word / Excel / HTML)**」で HTML / PDF / Word / Excel へ変換
- **プレビュー編集中は、エディタタブを右クリック →「ファイルを変換」** —— いま開いている文書がそのまま変換されます
- フォルダを右クリック →「**フォルダ内のファイルを一括変換**」でまとめて変換

## インストール

入手経路は 3 つあります。お使いのエディタが読むものをお選びください。

**拡張機能ビューから（VS Code）**

拡張機能ビュー（Ctrl+Shift+X）で `GlyphCove` を検索し、「**インストール**」を選びます。コマンドラインの場合:

```powershell
code --install-extension glyphcove.glyphcove
```

**Open VSX から（Cursor / VSCodium など）**

これらのエディタは Visual Studio Marketplace ではなく Open VSX レジストリを読みます。拡張機能ビューで `GlyphCove` を検索し、同じようにインストールします。

**`.vsix` をダウンロードして** —— どちらのストアにも接続できない環境向け

1. [リリースページ](https://github.com/aporo0711/glyphcove-docs/releases)から最新の `glyphcove-vX.Y.Z.vsix` をダウンロード
2. 拡張機能ビューを開き、右上の `…` メニュー →「**VSIX からのインストール...**」
3. ダウンロードした `.vsix` ファイルを選択

コマンドラインの場合:

```powershell
code --install-extension glyphcove-vX.Y.Z.vsix
```

> **更新について**: 拡張機能ビューと Open VSX から入れた場合は、VS Code の `extensions.autoUpdate`（既定でオン）が有効なあいだは自動で更新されます。**`.vsix` で入れた場合は自動更新されません** —— 新しいリリースの案内があった場合は、同じ手順で新しい `.vsix` をインストールしてください（上書きされます）。

不具合の報告・機能の要望は [Issue](https://github.com/aporo0711/glyphcove-docs/issues) へお願いします（書きかたは [CONTRIBUTING.md](../CONTRIBUTING.md) にあります）。

## もっと詳しく

- [FAQ（よくある質問）](guide/faq.ja.md)
- [プレビュー編集（WYSIWYG）](guide/editing.ja.md)
- [変換](guide/conversion.ja.md)
- [必要ランタイム・外部ツール](guide/requirements.ja.md)
- [セキュリティ](guide/security.ja.md)
- [既知の正規化・制限](guide/limitations.ja.md)
- [やらないことリスト](guide/not-planned.ja.md)
- 開発者向け: [アーキテクチャと ADR](architecture.ja.md)
- [バグ報告・機能要望の手引き](../CONTRIBUTING.md)
- [脆弱性の報告のしかた](../SECURITY.md)

## 料金の予定（事前のお知らせ）

**現在のバージョンでは、すべての機能を無料でお使いいただけます。** 後から突然変わることがないよう、将来のリリースで引く予定の線をあらかじめお知らせします。

- **無料のまま**: Markdown の編集・プレビュー・貼り付けと、HTML / PDF への基本出力
- **有償の Pro ライセンス（買い切り）の対象にする予定**: Word / Excel への出力（MD → DOCX / XLSX）、逆変換（PDF / Word / Excel / HTML → Markdown）とそれを土台にした 2 段変換、一括変換と結合、品質レポート、「Office 用にコピー」、印刷時の柱（ヘッダー/フッター・ページ番号）、透かし
- **Enterprise（組織向け）の対象にする予定**: ポリシー固定・調達向け資料

ライセンス認証は本製品の設計どおり**完全オフライン**で行います（ネットワーク送信はありません）。バージョンが明示するまでは、上記もすべて無料のままです。**「無料のまま」の枠をあとから狭めることはしません。**

## ライセンス

本拡張機能の利用には **[GlyphCove 使用許諾契約書](../LICENSE)**（版 1.2）が適用されます。全文は同梱の [LICENSE](../LICENSE) にあり、拡張機能の中からはコマンドパレットの「**GlyphCove: 使用許諾契約書を表示**」でいつでも開けます。

要点だけ挙げます（**下の要約は契約の一部ではありません**。効力を持つのは LICENSE の全文です）。

- **無償で、商用・非商用を問わず、期間の制限なく使えます**（第 4 条）。台数や人数の制限もありません
- **作った文書はすべてあなたのものです。** 変換・編集した成果物について当方はいかなる権利も主張しません。契約が終了した後も自由に使えます（第 3 条 3 項・第 20 条 3 項）
- **配布は 3 つの経路だけです** — Visual Studio Marketplace / Open VSX / 配布ページからの `.vsix` 直接ダウンロード（第 3 条 4 項）。インストール・バックアップのための複製は明示的に認めています（同 5 項）が、**再配布はできません**（第 9 条 1 号）
- **完全オフラインなので、当方が提供・サポートを終了した後も、取得済みのバージョンはそのまま使えます**（第 17 条 4 項）
- **本ソフトウェアはあなたのファイルを読み取り、変更し、新たに作成します。** 変換は原理的にロスがあります。**使う前にバックアップを取り、結果を検証してください**（第 12 条）
- 責任の制限は第 15 条にあります。**当方の故意または重大な過失による場合には上限は適用されません**

> ⚠ **この契約書はまだ弁護士のレビューを受けていません。** 有償版の発売までにレビューを受ける予定です。版と発効日は LICENSE の冒頭に記載しています。

依存 OSS のライセンス表示は `THIRD_PARTY_LICENSES.txt` として**パッケージ時に自動生成**され、`.vsix` に同梱されます（`npm run licenses` / `vscode:prepublish`）。リポジトリには置きません。
