# Changelog / 変更履歴

> **Every entry below is written in English first, then in Japanese.**
> **各項目は英語・日本語の順で併記しています。**

This file follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the
version numbers follow [Semantic Versioning](https://semver.org/).

このファイルの書式は [Keep a Changelog](https://keepachangelog.com/ja/1.1.0/) に、
バージョン番号は [セマンティック バージョニング](https://semver.org/lang/ja/) に従います。

## Unreleased / 未リリース

### Changed / 変更

- **We removed every encryption feature whose purpose is to keep information secret
  from the distributed package.**
  This extension bundles PDF.js in order to read PDFs, and that library carried the
  encryption and decryption defined by the PDF specification (AES-128 / AES-256 / RC4);
  the library we bundle in order to read ZIP archives carried the decryption for
  traditional ZIP encryption (ZipCrypto). Converting password-protected files is not
  something this extension sets out to do, so those implementations have been stripped
  out of what we ship. Hash functions (used for things such as computing a PDF
  identifier) and random number generation remain.

  **配布物から、情報の秘匿を目的とする暗号機能をすべて取り除きました。**
  本拡張機能は、PDF を読むために同梱している PDF.js に PDF 仕様の暗号化・復号
  （AES-128／AES-256／RC4）を、ZIP を読むために同梱しているライブラリに
  ZIP 従来型暗号（ZipCrypto）の復号を含んでいました。本拡張機能は
  パスワードで保護されたファイルの変換を対象としていないため、これらの実装を
  配布物から除去しました。ハッシュ関数（PDF の識別子計算などに使います）と
  乱数生成は残ります。

- ⚠ **As a consequence, password-protected PDFs can no longer be converted.**
  Until now a PDF that had only an owner password set, with an empty user password,
  was simply read as-is; from now on that case is not converted either, and we stop
  and tell you so.
  ⚠ **A PDF that does not ask you for a password when you open it is still protected
  if printing or copying is restricted.** Those cannot be converted either, so please
  remove the protection with software that can edit PDFs before using them here.

  ⚠ **これに伴い、パスワードで保護された PDF は変換できなくなりました。**
  従来は「オーナーパスワードのみが設定され、閲覧用パスワードが空の PDF」を
  そのまま読み取っていましたが、今後はこれも変換せず、その旨を表示して中止します。
  ⚠ **開くときにパスワードを求められない PDF でも、印刷やコピーが制限されていれば
  保護されています。**この場合も変換できませんので、PDF を編集できるソフトで
  保護を解除してからご利用ください。

- **We revised the licence agreement to version 1.2 (Article 21, export control).**
  Article 21(3) of version 1.0 stated that the software "has no encryption feature
  intended to keep information secret", which did not match the facts, so version 1.1
  corrected it to match reality. Following the removal described above, version 1.2
  again states that the software contains no encryption or decryption intended to keep
  information secret, and that it does not handle password-protected files. Two things
  are unchanged: that we will present, on request, a classification statement recording
  the result and the grounds of the export-control assessment under the Foreign Exchange
  and Foreign Trade Act, and that the licence-key verification feature (once we start
  offering it) only verifies a digital signature.

  **使用許諾契約書を版 1.2 に改定しました（第21条・輸出管理）。**
  版 1.0 の第21条3項は「情報の秘匿を目的とする暗号化機能を有しません」と述べて
  いましたが、これは事実と異なっていたため版 1.1 で事実に合わせて訂正しました。
  上記の除去を受けて、版 1.2 では改めて「秘匿を目的とする暗号化・復号の機能を
  含まない」ことと、パスワード保護されたファイルを取り扱わないことを定めています。
  外為法上の該非判定の結果と根拠を記した該非判定書を請求により提示する点、および
  ライセンスキーの検証機能（提供開始後）が電子署名の検証のみを行う点は変わりません。

- **We revised the licence agreement to version 1.3 (Article 16, support).**
  Article 16(3)(iii) of version 1.2 stated that we do not provide support in languages
  other than Japanese, while the guidance placed at our contact point invited reports in
  English as well: the agreement and the contact point disagreed. Version 1.3 states that
  **we accept both Japanese and English, and reply in the same language as the enquiry.**

  **使用許諾契約書を版 1.3 に改定しました（第16条・サポート）。**
  版 1.2 の第16条3項(3) は「日本語以外の言語による対応」を行わないと定めていましたが、
  問い合わせ窓口に置いた案内は英語での報告も受け付けており、契約と窓口が食い違って
  いました。版 1.3 では**日本語と英語の両方を受け付け、お問い合わせに使われた言語と
  同じ言語で回答する**ことを定めています。

- **The same revision removed the "first response within two business days" promised to
  the Enterprise edition.**
  The contact point is public issues only and cannot identify who holds a contract, so
  the clause stood as an unconditional obligation with no means of performing it. When we
  begin offering Enterprise we will prepare a private channel of contact first, and only
  then set out a rule about response times again.
  ⚠ Enterprise is not being offered yet.

  **同じ改定で、Enterprise エディションへの「2 営業日以内の一次応答」を削除しました。**
  窓口が公開 Issue のみで契約者を識別できないため、履行の手段がないまま無条件の債務と
  して書かれていたためです。Enterprise の提供を開始する際に、非公開の連絡経路を用意した
  うえで、応答についての定めを改めて設けます。⚠ Enterprise の提供はまだ開始していません。

- **We now ship the Japanese documentation inside the extension, and you can open it from
  a command.**
  Until now not a single Japanese guide was included in the package, the description shown
  in the Extensions view was English only, and the Japanese documentation could only be
  reached through a URL on the public mirror. For a product that makes a point of working
  fully offline, having only the Japanese documentation depend on the network was wrong, so
  the guides are now bundled in both Japanese and English and can be opened from the
  command palette with **"GlyphCove: Open the user guide"**.

  **日本語の説明を拡張機能に同梱し、コマンドから開けるようにしました。**
  これまで配布物に日本語の手引きは 1 本も入っておらず、拡張機能ビューに出る説明も
  英語だけで、日本語の説明へは公開ミラーの URL からしか到達できませんでした。
  完全オフラインで動くことを掲げているのに日本語の説明だけがネットワーク前提だったため、
  日本語版・英語版の手引きを同梱し、コマンドパレットの
  **「GlyphCove: 使用手引きを開く」**から選んで開けるようにしました。

- **This changelog is now written in both English and Japanese.**
  The Changelog tab on the marketplace can only hold one file, and there is no way to
  serve a different language to different readers, so every entry is now written in
  English first and Japanese second. Until now the tab was Japanese only, while the
  description beside it was English only.

  **この変更履歴を、日本語と英語の両方で書くようにしました。**
  店頭の Changelog タブは 1 本しかファイルを持てず、読む人によって言語を出し分ける
  こともできないため、すべての項目を英語・日本語の順で併記しました。これまでは
  タブの中身が日本語だけで、隣に並ぶ説明は英語だけ、という状態でした。

### Fixed / 修正

- **In Excel conversion, the absolute path of the input file was written into the
  `_Metadata` sheet.**
  Handing someone a converted .xlsx let them see **the user name and folder layout of
  your machine**. We now write only the file name.

  **Excel 変換で、`_Metadata` シートに入力ファイルの絶対パスが書き込まれていました。**
  変換した .xlsx を人に渡すと、受け取った相手に**お使いの環境のユーザー名とフォルダ構成**が
  見える状態でした。ファイル名だけを書くように改めました。

### Added / 追加

- **We added two-stage conversion across formats, by way of Markdown.**
  That adds ten routes: PDF → Word / Excel / HTML, Word → PDF / Excel,
  Excel → PDF / Word, and HTML → PDF / Word / Excel. It works by chaining the
  X → Markdown and Markdown → Y converters automatically, and the names of the choices say
  so — "PDF → Word (via Markdown; the layout is not reproduced)". What carries over is the
  structure of the document; the original layout is not reproduced. The result comes with a
  quality report that gathers the warnings from both stages, and states at the top that the
  conversion went through two stages (the first stage's quality report is included in the
  same file). Word → HTML and Excel → HTML are not offered through this route, because
  putting Markdown in the middle loses things both ends can express, such as merged cells.

  **形式をまたぐ 2 段変換（Markdown 経由）を追加しました。**
  PDF → Word / Excel / HTML、Word → PDF / Excel、Excel → PDF / Word、
  HTML → PDF / Word / Excel の 10 通りが増えます。X → Markdown と
  Markdown → Y の変換器を自動で連結する方式で、選択肢の名前も
  「PDF → Word（Markdown 経由・レイアウトは再現しません）」——
  引き継がれるのは文書の構造で、元のレイアウトは再現しません。
  変換結果には両段の警告をまとめた品質レポートが付き、冒頭に 2 段で
  変換したことを明記します（第 1 段の品質レポートも同じファイルに収録）。
  Word → HTML と Excel → HTML はこの経路では提供しません
  （Markdown を挟むと、両端が表現できる結合セルなどが失われるため）

- **We added direct Word → HTML and Excel → HTML conversion (it does not go through
  Markdown).**
  Both **keep merged table cells as colspan / rowspan** — exactly what putting Markdown in
  the middle always loses. On the Word side, headings, lists, quotations, code and task
  lists keep their structure in the HTML; on the Excel side, sheets become HTML tables that
  keep the grid (values as they look in Excel, hyperlinks still links, hidden sheets
  marked). In both cases images are written out as real files into `<output name>_images/`
  (nothing is embedded as base64). Word → HTML comes with the same quality report
  (JSON / MD) as DOCX → Markdown, recording the text of headers, footers and comments that
  could not be carried into the output.

  **Word → HTML と Excel → HTML の直接変換を追加しました（Markdown を経由しません）。**
  どちらも**表の結合セルを colspan/rowspan のまま保持**します —
  Markdown を挟むと必ず失われるものです。Word 側は見出し・リスト・引用・
  コード・タスクリストを構造のまま HTML に、Excel 側はシートを格子のまま
  HTML の表にします（値は Excel の見た目・ハイパーリンクはリンクのまま・
  非表示シートは印つき）。画像はどちらも `<出力名>_images/` への
  実ファイル書き出しです（base64 埋め込みなし）。
  Word → HTML には DOCX → Markdown と同じ品質レポート（JSON/MD）が付き、
  出力へ出せなかったヘッダー/フッター・コメントの本文を収録します

- **We added HTML → Markdown conversion** (`.html` / `.htm`).
  It uses the same engine as pasting from the web: the site's furniture (navigation,
  headers, footers), styles and scripts are dropped and only the body is converted. Tables
  are rebuilt as Markdown tables, and expanding merged cells and flattening nested tables
  are reported as warnings. Images are neither copied nor embedded — the references are
  left as written (and if the output goes to a different folder, we warn you about that).
  Files are read as UTF-8, and a document that declares a different character encoding
  raises a warning.

  **HTML → Markdown 変換を追加しました**（`.html` / `.htm`）。
  Web からの貼り付けと同じエンジンで、サイトの飾り（ナビゲーション・
  ヘッダー・フッター）とスタイル・スクリプトを除いて本文だけを変換します。
  表は Markdown の表に再構成し、結合セルの展開と入れ子の表の平坦化は
  警告で報告します。画像はコピーも埋め込みもせず、参照を書かれたまま
  残します（出力先が別フォルダのときは、その旨を警告します）。
  ファイルは UTF-8 として読み、別の文字コードを宣言している文書には
  警告を出します

### Changed / 変更

- **We documented, in the guides, that image width cannot be changed with the keyboard
  alone.**
  The handles at the four corners of an image in the preview respond only to dragging, and
  there is no alternative by keyboard, nor by a single pointer press-and-release (so we do
  not meet WCAG 2.2 success criteria 2.1.1 Keyboard, 2.5.7 Dragging Movements, or 2.5.8
  Target Size (Minimum)). None of our documents had said so, so we added a note to
  [Preview editing](docs/guide/editing.md) and
  [Known normalisations and limitations](docs/guide/limitations.md), together with the
  steps for setting the width as a number (open the source side by side with **"Open
  source"** on the toolbar, and edit the `width` in
  `<img src="…" alt="…" width="480">` directly).
  ⚠ **A way to do it other than the handles is not implemented yet.**

  **画像の幅の変更がキーボードだけではできないことを、ガイドに明記しました。**
  プレビュー上の画像の四隅のハンドルはドラッグ操作にしか反応せず、キーボードにも、
  押して離すだけの単一ポインタ操作にも代替がありません（WCAG 2.2 の
  2.1.1「キーボード」・2.5.7「ドラッグ動作」・2.5.8「ターゲットのサイズ（最小）」を
  満たしていません）。これまでどの文書にも書いていなかったため、
  [編集機能](docs/guide/editing.ja.md)と[既知の正規化・制限](docs/guide/limitations.ja.md)に
  断り書きを入れ、幅を数値で指定する手順を書きました
  （ツールバーの **「ソースを開く」** でソースを横に並べて開き、
  `<img src="画像のパス" alt="代替テキスト" width="480">` の `width` を直接書き換える）。
  ⚠ **ハンドル以外の操作手段は、まだ実装していません。**

- **A document being edited in the preview can now be converted as it is.**
  Until now, running "Convert file" from the command palette always brought up a file
  picker, even when the document was open in the preview editor, and you had to select the
  document you were already editing. If the frontmost tab is a file we can convert, that
  file becomes the target (running it from the explorer or a tab's context menu behaves as
  before).

  **プレビュー編集中の文書を、そのまま変換できるようになりました。**
  これまでコマンドパレットから「ファイルを変換」を実行すると、プレビュー編集で
  文書を開いていても必ずファイル選択ダイアログが出て、いま編集している文書を
  自分で選び直す必要がありました。前面のタブが変換できるファイルであれば、
  そのファイルが変換対象になります（エクスプローラーやタブの右クリックから
  実行したときの動きは変わりません）。

- **We wrote the installation steps for all three ways of getting the extension.**
  The README explained only how to install a `.vsix` directly, which made it a detour for
  anyone who had installed from the Extensions view. We now describe the Extensions view,
  Open VSX and downloading the `.vsix` directly, side by side, and limited the note about
  "it will not update automatically" to the `.vsix` case.

  **インストール手順を 3 つの入手経路すべてについて書きました。**
  README のインストール手順が `.vsix` を直接入れる方法だけを説明していたため、
  拡張機能ビューから入れた方が読む指示としては遠回りでした。拡張機能ビュー・
  Open VSX・`.vsix` の直接ダウンロードの 3 つを併記し、
  「自動更新されません」の注記を `.vsix` で入れた場合に限定しました。

- **We added two conversions that were missing from the table** — Word (DOCX) → HTML and
  Excel (XLSX) → HTML. Both have been available as direct conversions for some time, but
  they were absent from the tables in the README and on the site (nothing about the
  features themselves changed).

  **変換の一覧表に、抜けていた 2 つの変換を加えました** ——
  Word（DOCX）→ HTML と Excel（XLSX）→ HTML です。どちらも以前から
  使える直接変換ですが、README とサイトの表には載っていませんでした
  （機能そのものに変更はありません）。

- **We brought the description of behaviour in untrusted folders (Workspace Trust) into
  line with reality.**
  The README said "PDF conversion is disabled", but in fact we do not convert at all, nor
  render diagrams in the preview or in "Copy for Office".

  **未信頼のフォルダー（Workspace Trust）での動作について、説明を実態に合わせました。**
  README は「PDF 変換を無効化します」と書いていましたが、実際には変換も、
  プレビューや「Office 用にコピー」での図の描画も行いません。

- **Batch conversion of a folder now also covers files other than Markdown (PDF / Word /
  Excel).**
  Until now, right-clicking a folder could batch-convert Markdown only, and choosing a
  folder that held only PDFs ended with "There are no Markdown files in the folder." We now
  look for the same five kinds that batch conversion from a file selection accepts
  (md / markdown / pdf / docx / xlsx). For a folder holding a mixture, we ask which kind to
  convert, on the same screen as when you select files. The command is now named
  "Batch convert files in a folder".

  **フォルダの一括変換が、Markdown 以外のファイル（PDF / Word / Excel）も対象になりました。**
  これまでフォルダを右クリックして一括変換できるのは Markdown だけで、
  PDF だけのフォルダを選ぶと「フォルダ内にMarkdownファイルがありません。」で
  終わっていました。ファイル選択の一括変換が受け付ける 5 種類
  （md / markdown / pdf / docx / xlsx）を、フォルダでも同じように探します。
  種類の違うファイルが混ざっているフォルダでは、どの種類を変換するかを
  ファイル選択のときと同じ画面で尋ねます。
  コマンド名は「フォルダ内のファイルを一括変換」に変わりました

- **The formatting toolbar can now be passed with a single Tab press from the keyboard.**
  Until now you had to press Tab as many times as there were buttons (35) to reach the body
  text. The toolbar as a whole is now worth one Tab, and you move within it using ← → and
  Home / End (buttons that cannot be pressed right now are skipped). Esc returns you to the
  body. This is the same behaviour as VS Code's own toolbars.

  **書式ツールバーを、キーボードで 1 回のタブ移動で通り過ぎられるようにしました。**
  これまではボタンの数だけ（35 回）タブを押さないと本文にたどり着けませんでした。
  ツールバー全体がタブ 1 回ぶんになり、中の移動は ← → と Home / End で行います
  （今は押せない状態のボタンは飛ばします）。Esc で本文に戻ります。
  VS Code 自身のツールバーと同じ操作です

- ★ **We unified the namespace of settings and commands to `glyphcove.*`.**
  VS Code **derives the labels in the settings UI mechanically from the setting ID**, so the
  namespace becomes wording that users read. Settings now appear as, for example,
  "**Glyphcove › Conversion › Timeout Ms**". Not one setting changed in meaning, default or
  behaviour.

  ★ **設定とコマンドの名前空間を `glyphcove.*` に統一しました。**
  VS Code は設定画面の項目名を**設定 ID から機械的に作る**ため、名前空間はそのまま
  利用者が読む語になります。設定は「**Glyphcove › Conversion › Timeout Ms**」のように
  表示されます。設定の意味・既定値・振る舞いは 1 つも変わりません

- **Editing and preview now work in untrusted folders (Restricted Mode) as well.**
  Until now we did not declare what we support, so VS Code followed its default and
  **disabled the whole extension**. In Restricted Mode we **stop file conversion and
  diagram rendering** — both of them start an external program (the browser used for PDF
  output, the Java used for PlantUML). The body of editing and preview works as usual.
  Alongside this, the three settings that point at the locations of those programs cannot be
  changed from the folder side.

  **信頼していないフォルダー（制限モード）でも、編集とプレビューが使えるように
  なりました。** これまでは対応状況を宣言していなかったため、VS Code の既定に従って
  **拡張機能ごと無効**になっていました。制限モードでは、**ファイルの変換と図の描画を
  止めます** —— どちらも外部のプログラム（PDF 出力に使うブラウザー、PlantUML 用の
  Java）を起動するためです。編集とプレビューの本文はそのまま使えます。併せて、
  それらのプログラムの場所を指す 3 つの設定は、フォルダー側から変更できません

- **In virtual workspaces (vscode.dev, browsing a GitHub repository, and similar, where
  there are no files on disk) we no longer show the conversion menus.**
  Conversion reads and writes files on disk, so it cannot run there. Until now the menus
  appeared, and we went on to assemble a path to a file that was not on disk and tried to
  read it. Editing and preview work as before.

  **仮想ワークスペース（vscode.dev や GitHub リポジトリの閲覧など、ディスク上に
  ファイルが無い状態）で、変換のメニューを出さないようにしました。** 変換は
  ディスク上のファイルを読み書きするため、そこでは実行できません。これまでは
  メニューが出たうえで、ディスク上に無いファイルのパスを組み立てて読みに行っていました。
  編集とプレビューはこれまでどおり使えます

### Fixed / 修正

- **We fixed a case where an edited paragraph and the paragraph immediately below it were
  joined into one in the saved file.**
  With two blocks adjacent without a blank line between them (a `# heading` and a paragraph
  written on the next line, for instance), turning the upper block into an ordinary
  paragraph left **two paragraphs on screen but one paragraph in the file**. When they
  cannot be kept apart otherwise, we now write a blank line (that one place gains a line).

  **編集した段落と、そのすぐ下の段落が、保存したファイルの中で 1 つに
  つながってしまうことがあったのを直しました。** 空行を挟まずに隣り合っていた
  2 つのブロック（`# 見出し` と、その次の行に書かれた段落など）で、
  上のブロックを普通の段落に変えると、**画面では 2 つの段落のまま、
  ファイルでは 1 つの段落**になっていました。分けられない間隔のときは
  空行を書きます（そこだけ 1 行増えます）

- **We fixed blocks being rewritten when they had merely been reordered.**
  Swapping the order of paragraphs or code blocks rewrote only the one that moved, causing
  changes such as a `~~~` code fence becoming ```` ``` ````, or a quotation written over
  several lines being folded into one. Reordering no longer changes the bytes of the
  content.

  **ブロックを並べ替えただけのときに、動かしたブロックが書き直されていたのを
  直しました。** 段落やコードブロックの順番を入れ替えると、動かしたほうだけが
  書き直され、`~~~` のコードフェンスが ``` に変わる・折り返して書いた引用が
  1 行にまとまる、といった変化が起きていました。並べ替えでは中身のバイトは
  変わりません

- **When a conversion times out, we now say what to do next.**
  Until now it ended at "Timed out (120000ms)". We now distinguish between not having
  enough time for a large document and the case where it happens on every PDF conversion
  and raising the limit does not help, and we attach a button that opens the time setting
  there and then.

  **変換がタイムアウトしたときに、次に何をすればよいかを出すようにしました。**
  これまでは「タイムアウトしました（120000ms）」で終わっていました。大きな文書で
  時間が足りないのか、PDF 変換のたびに毎回起きていて時間を延ばしても直らないのかを
  書き分け、時間の設定をその場で開けるボタンを添えます

- **You can now check a stored licence key on screen, and deleting one asks for
  confirmation.**
  Until now "Show licence status" did not show the key, and a single press of "Delete"
  removed it irreversibly. The status display now shows part of the key (the edition and
  the last five characters), so you can confirm which key you are about to delete.

  **保存したライセンスキーを画面で確かめられるようにし、削除するときに確認を
  挟むようにしました。** これまでは「ライセンスの状態を表示」に鍵が出ず、
  「削除」を 1 回押すだけで取り消せずに消えていました。状態の表示に鍵の一部
  （版種と末尾 5 文字）が出るようになり、削除の前にどの鍵を消すのかを確かめられます

- **We fixed "dead links" to images that could not be embedded being left in the exported
  SVG when pasting a stencil.**
  draw.io (26.0.2) turns an image it could not embed into a `http://127.0.0.1:…` URL that is
  valid only while it is drawing, so a reference that cannot be read later was left in the
  file. We now remove that reference (the rest of the diagram is untouched) and tell you in
  a warning that we removed it.

  **ステンシルの貼り付けで、図に埋め込めなかった画像への「死んだリンク」が
  書き出した SVG に残ることがあったのを直しました。**
  draw.io（26.0.2）は埋め込めなかった画像を描画時だけ有効な
  `http://127.0.0.1:…` の URL にするため、あとで開いても読めない参照が
  ファイルに残っていました。この参照は取り除き（図のほかの部分は不変）、
  取り除いたことを警告でお知らせします

- **In "Copy for Office", when diagram rendering ran out of time, the same warning was
  repeated for each kind (Mermaid / PlantUML); it is now issued once.**
  We also stopped saying that an SVG image which did not finish loading within the time
  limit was "not found" (we do not know whether it could have been read).

  **「Office 用にコピー」で、図の描画が時間切れになったとき、同じ理由の警告が
  種別ごと（Mermaid / PlantUML）に重複して出ていたのを 1 回にしました。**
  あわせて、時間切れで読み終わらなかった SVG 画像を「見つからない」と
  言っていたのを改めました（読めたかどうか分からないため）

- **In "Copy for Office", images written as raw HTML with an absolute path
  (`<img src="C:\…">`) vanished from the paste without any warning; this is fixed.**
  They are now converted to a file: URL before being handed over, exactly like images in
  Markdown notation (raw HTML images with relative paths worked before and still do).

  **「Office 用にコピー」で、生の HTML で書いた `<img src="C:\…">`（絶対パス）の
  画像が、警告も出ないまま貼り付けから消えていたのを直しました。**
  Markdown 記法の画像と同じように file: URL へ変換してから渡します
  （相対パスの生 HTML 画像は従来どおり動きます）

- **In "Copy for Office", when an SVG image could not be read, the reason appeared nowhere;
  this is fixed.**
  An unreadable `.svg` image — not found, a web URL, a path on another machine — is replaced
  by a note, but the reason for it did not appear in the notification (measured: referencing
  a non-existent `missing.svg` in image notation produced zero warnings). We now report
  which file could not be read and why, in a warning.

  **「Office 用にコピー」で、SVG 画像が読めなかったときに理由がどこにも出なかったのを直しました。**
  見つからない・Web の URL・別の機械のパスなど、読めない `.svg` 画像は
  断り書きに置き換わりますが、その理由は通知に出ていませんでした
  （存在しない `missing.svg` を画像記法で参照して、警告 0 件を実測）。どのファイルがなぜ読めなかったかを
  警告で報告します

- **We fixed SVG images carrying a query or fragment, such as `x.svg?v=1`, not being treated
  as diagrams by "Copy for Office" and being pasted as an SVG reference Word cannot draw.**
  The check for the file extension looked only at the end of the target.

  **`x.svg?v=1` のようにクエリやフラグメントの付いた SVG 画像が、
  「Office 用にコピー」で図として扱われず、Word が描けない SVG 参照のまま
  貼り付いていたのを直しました。** 拡張子の判定が行き先の末尾だけを見ていました

- **We fixed the warning issued when diagrams exceed the limit, which asserted "we turned
  only {count} of them into images".**
  That warning is issued before drawing, so it does not know the outcome, and it said "we
  turned them into images" even on runs where not a single one could be. It now reads "we
  attempted only the first {count}".

  **図が上限を超えたときの警告が「{count} 件だけ画像にしました」と
  言い切っていたのを直しました。** この警告は描く前に出るので結果を知らず、
  1 枚も画像にできなかった回にも「画像にしました」と出ていました。
  「先頭の {count} 件だけを試しました」に改めました

- **The temporary PNG directory used by "Copy for Office" is now created with permissions
  that let only its owner read it (0700).**
  Until now it used the default permissions (usually 0755), so on a shared Linux or macOS
  machine other users could read the diagrams you had copied. Nothing changes on Windows.

  **「Office 用にコピー」の一時 PNG の置き場を、所有者だけが読める
  権限（0700）で作るようにしました。** これまでは既定の権限（通常 0755）で、
  Linux / macOS の共用機では他の利用者がコピーした図を読める状態でした。
  Windows では変化ありません

- **We fixed instructions planted in a diagram's SVG being able to make an outbound
  connection.**
  When turning a diagram into a PNG, we used to **embed that SVG directly** into a working
  page. A `.svg` file referenced by the document is read without passing through the
  sanitiser (draw.io puts its labels in a `foreignObject`, and sanitising drops them), so
  **writing `<meta http-equiv="refresh">` inside a `foreignObject` made that page navigate
  to an external URL**. The working page carried a `default-src 'none'` CSP, but **CSP has
  no directive that stops navigation** (we also measured that scripts and external images
  were indeed blocked). We changed it to load the SVG as an `<img>`, and confirmed on a real
  machine that it does not navigate.
  ⚠ This concerns "Copy for Office" and embedding diagrams into DOCX / XLSX.
  ⚠ **It does not happen on the default rendering path** — only on the fallback path used
  when that one fails.

  **図の SVG に仕込まれた指示で、外部へ接続してしまうことがあったのを直しました。**
  図を PNG にするとき、これまではその SVG を作業用ページに**直接埋め込んで**いました。
  文書に書いた `.svg` ファイルはサニタイザを通さずに読むため（draw.io のラベルが
  `foreignObject` にあり、サニタイズすると落ちるため）、
  **`foreignObject` の中に `<meta http-equiv="refresh">` を書いておくと、
  そのページが外部の URL へ遷移します**。作業用ページには `default-src 'none'` の
  CSP を張っていましたが、**CSP には遷移を止める指示がありません**
  （スクリプトと外部画像は止まっていることも実測で確認しています）。
  SVG を `<img>` として読み込む形に変え、遷移しないことを実機で確かめました。
  ⚠ 対象は「Office 用にコピー」と DOCX / XLSX への図の埋め込みです。
  ⚠ **既定の描画経路では起きません** — 起きるのは、その経路が失敗したときに
  使う予備の経路だけでした

- **We fixed the body text disappearing entirely from Japanese PDFs that do not embed their
  fonts.**
  Documents with "non-embedded CID fonts" (standard encodings such as `90ms-RKSJ-H`), which
  are common in government and corporate PDFs, need the byte-to-character mapping table
  (CMap) to be read from the bundled `.bcmap` files. We were passing the location of those,
  but **we passed it as a `file://` URL, so pdf.js could not read a single one of them**
  (pdf.js on the Node side reads with `fs.readFile`, so a URL string is treated as a path
  and always fails). **It does not raise an error; those characters just vanish silently.**
  ⚠ Impact: **missing body text** in Japanese PDF → Markdown / Word / Excel, and pages
  judged to have "too few characters" being **mistaken for scans**. The same mapping table
  is needed when rasterising vector diagrams, so we now pass it there too (labels in
  diagrams were coming out as blank white shapes).

  **フォントを埋め込んでいない日本語 PDF から、本文が丸ごと消えていたのを直しました。**
  官公庁・企業の PDF に多い「CID フォント非埋め込み（`90ms-RKSJ-H` などの
  標準エンコーディング）」の文書は、バイトから文字への対応表（CMap）を
  同梱の `.bcmap` から読む必要があります。その置き場は渡していたのですが、
  **`file://` の URL 形式で渡していたため、pdf.js が 1 ファイルも読めていませんでした**
  （Node 側の pdf.js は `fs.readFile` で読むので、URL 文字列はパスとして扱われて
  必ず失敗します）。**エラーにはならず、その文字だけが黙って消えます。**
  ⚠ 影響: 日本語 PDF → Markdown / Word / Excel の**本文欠落**、
  および「文字が少ない」と判定されたページの**スキャン誤判定**。
  ベクタ図の画像化でも同じ対応表が要るので、そちらにも渡すようにしました
  （図のラベルだけが白く抜けた画像になっていました）

- **We fixed PlantUML's "error image" being embedded into the conversion result as though it
  were a normal diagram when PlantUML terminated abnormally.**
  PlantUML reports syntax errors on stderr (`-stdrpt`), but **when PlantUML itself terminates
  abnormally that report does not appear, and it emits the error image as usual** (with exit
  code 0 as well). As a result PlantUML's own wording — "An error has occured", "Sorry, the
  subproject …" — and **a QR code with the diagram's source embedded in it** went straight
  into the HTML / PDF / Word / Excel. We now treat a Java stack trace on stderr as evidence
  of abnormal termination, discard the image, and report "PlantUML terminated abnormally
  (exception class name)".
  ⚠ **We neither record nor display the exception's message** (it may quote the contents of
  the document). Note that this defect depends on the version of the jar (reproduced with
  1.2024.3, resolved in 1.2026.6).

  **PlantUML が異常終了したとき、その「エラー画像」が正常な図として
  変換結果に埋め込まれていたのを直しました。** PlantUML は構文エラーを
  stderr で報告しますが（`-stdrpt`）、**PlantUML 自身が異常終了した場合は
  その報告が出ないまま、エラー画像を通常どおり出力します**（終了コードも 0）。
  そのため「An error has occured」「Sorry, the subproject …」という
  PlantUML 側の文言と、**図のソースを埋め込んだ QR コード**が、
  そのまま HTML / PDF / Word / Excel に入っていました。
  stderr の Java スタックトレースを異常終了の証拠として見るようにし、
  画像は捨てて「PlantUML が異常終了しました（例外クラス名）」と報告します。
  ⚠ 例外の**メッセージは記録も表示もしません**（文書の中身を引用しうるため）。
  なお、この不具合は jar の版によります（1.2024.3 で再現、1.2026.6 では解消）

- **We fixed PDF → Markdown discarding the entire body of pages that hold few characters.**
  Pages with fewer than 20 characters were regarded as having no text layer (a scan) and
  their body was thrown away. Measured on a real document, of the 14 pages treated as scans
  **6 did have characters, and their content was the chapter title pages** (for example
  `第Ⅰ章 監視カメラ／システム総市場`, 4 to 17 characters). Any page with even one character
  now goes into the body, and the facts that it "may be a scan" and that "the images on this
  page have not been extracted" are reported as `PDF_SPARSE_PAGE`. The quality report carries
  the count too.

  **PDF → Markdown で、文字数の少ないページの本文が丸ごと消えていたのを直しました。**
  20 文字に満たないページは「テキストレイヤーが無い（スキャン）」と見なして
  本文を捨てていました。実文書で測ると、スキャン扱いの 14 ページのうち
  **6 ページは文字を持っていて、その中身は章扉の見出し**でした
  （`第Ⅰ章 監視カメラ／システム総市場` など。4〜17 文字）。
  文字が 1 つでもあるページは本文に出すようにし、
  「スキャンの可能性がある／このページの画像は取り出していない」ことは
  `PDF_SPARSE_PAGE` で報告します。品質レポートにも件数を載せます

- **We fixed one of two files that differ only in case not being converted on Linux when
  they were converted together** (`Report.md` and `report.md`).
  The check for colliding output paths was not case-sensitive. The default file systems on
  Windows and macOS are not case-sensitive, so nothing changes there.

  **Linux で、大文字小文字だけが違う 2 つのファイルをまとめて変換すると
  片方が変換されなかったのを直しました**（`Report.md` と `report.md`）。
  出力先の衝突判定が大文字小文字を区別していませんでした。
  Windows と macOS の既定のファイルシステムは区別しないので、そこは従来どおりです

- **On Linux and macOS we now also look on the PATH for the browser used to print PDFs.**
  Assuming it can only be in a fixed location is a Windows habit; distributions, Homebrew
  and Flatpak all put it somewhere different.

  **Linux / macOS で、PDF 印刷に使うブラウザを PATH からも探すようにしました。**
  決まった場所にしか無いという前提は Windows の流儀で、
  ディストリビューションや Homebrew / Flatpak では置き場が違います

- **We fixed chapter order being able to change with the terminal's language settings when
  several Markdown files are combined into one.**
  The rule for ordering file names followed **the machine's default locale**, so the same
  files could produce **a different chapter order depending on who ran the conversion**. The
  ordering rule is now fixed, so from now on everyone gets the same order.
  ⚠ **Output does not change in Japanese or English environments.** Numeric ordering
  (`2-x.md` → `10-x.md`) is the same under every language setting, and that is the main
  purpose of this feature. The order changed only for file names differing solely by accented
  characters, letter case or digraphs, on machines set to Swedish, Danish, Czech, Turkish,
  Chinese and the like (in Swedish, for instance, `ä` sorts after `z`). The **processing order
  and the order of the conversion log** in batch conversion use the same rule.

  **複数の Markdown を 1 つにまとめるとき、章の順番が端末の言語設定によって
  変わりうるのを直しました。** ファイル名を並べる規則が**端末の既定ロケール**に
  従っていたため、同じファイルを渡しても**変換する人によって章順が変わりえます**でした。
  並べる規則を固定したので、これからは誰が変換しても同じ順序になります。
  ⚠ **日本語・英語の環境では出力は変わりません。** 数字の並び
  （`2-x.md` → `10-x.md`）はどの言語設定でも同じで、そこがこの機能の主目的だからです。
  順序が変わっていたのは、アクセント付きの文字・大文字小文字・二重字だけが違う
  ファイル名を、スウェーデン語やデンマーク語、チェコ語、トルコ語、中国語などの
  設定の端末で扱った場合です（例: スウェーデン語では `ä` が `z` の後ろに並びます）。
  一括変換の**処理順と変換ログの並び**も同じ規則を使っています

## 0.6.0 - 2026-08-16

The first baseline. What worked at this point:

初回のベースライン。この時点で動くもの:

### Preview editing / プレビュー編集

- Editing `.md` directly on the preview screen (paragraphs, headings, emphasis, links,
  bullet / numbered / task lists, quotations, code blocks, horizontal rules, tables).

  `.md` をプレビュー画面上で直接編集（段落・見出し・装飾・リンク・
  箇条書き / 番号 / タスクリスト・引用・コードブロック・水平線・表）

- Block-by-block partial serialisation that does not rewrite a single byte of blocks you
  have not edited. Syntax we do not support is not silently dropped; it is kept as a
  read-only block.

  編集していないブロックは 1 バイトも書き換えないブロック単位の部分シリアライズ。
  未対応の構文は黙って消さず、読み取り専用ブロックとして保持する

- Search and replace, a table of contents (outline), raw YAML editing of front matter, and
  broken links shown in the Problems panel.

  検索・置換、目次（アウトライン）、front matter の生 YAML 編集、
  壊れたリンクの「問題」パネル表示

- Pasting, dragging and dropping, and inserting images (saved as external files; never
  turned into base64).

  画像の貼り付け・ドラッグ&ドロップ・挿入（外部ファイルとして保存。base64 化はしない）

- Pasting tables from Excel, Word and web pages, and tables drawn with ruled lines.

  Excel / Word / Web ページの表、罫線で描かれた表の貼り付け

- Rendering Mermaid, PlantUML and draw.io diagrams in the preview.

  Mermaid・PlantUML・draw.io の図をプレビューに描画

### Conversion / 変換

- Markdown → HTML / PDF / Word (DOCX) / Excel (XLSX).

  Markdown から HTML / PDF / Word（DOCX）/ Excel（XLSX）へ変換します。

- PDF / Word / Excel → Markdown (every loss in conversion is reported as a warning).

  PDF / Word / Excel → Markdown（変換のロスはすべて警告として報告する）

- Batch conversion by folder, and conversion logs (JSON / text).

  フォルダ単位の一括変換と、変換ログ（JSON / テキスト）の出力

- Rich copy for pasting into Office.

  Office へ貼り付けるためのリッチコピー

### Principles / 方針

- No telemetry. Nothing is ever sent anywhere.

  テレメトリなし。外部への送信は一切行いません

- External `http(s)` images are not loaded in the preview.

  外部 `http(s)` 画像はプレビューで読み込みません

- Losses and omissions in conversion are shown honestly in the UI and the log (we do not
  dress up a failure as a success).

  変換のロス・欠落は UI とログに正直に表示します（成功を装わない）
