# Security Policy for GlyphCove

**日本語は[後半](#glyphcove-のセキュリティ報告について)にあります。**

## Reporting a vulnerability

If you believe you have found a security vulnerability in GlyphCove, please
**do not open a public issue** — that would put the method in front of everyone
while no fix exists yet.

Use GitHub's private vulnerability reporting on the
[glyphcove-docs repository](https://github.com/aporo0711/glyphcove-docs):
**Security → Report a vulnerability**. Only the maintainers can read what you
write there. This is the only channel for security reports.

## What to include

GlyphCove collects no usage data at all, so **we have no way to look up what
happened on your machine.** Everything we know comes from your report.

- What you did, and what an attacker could do as a result
- Steps to reproduce, ideally with a **minimal sample document** — please
  remove anything confidential before attaching it
- The GlyphCove version (Extensions view → GlyphCove), the VS Code version
  (Help → About), and your OS
- Any external tool involved and its version (the Java that runs PlantUML,
  the browser used for PDF conversion, and so on)

## What happens next

- We aim to send a first reply within **5 business days**. GlyphCove is
  developed by one person, so that is a target rather than a guarantee. If
  ten business days pass with no reply, please send a reminder through the
  same private report.
- We will tell you whether we could reproduce the problem, and what we intend
  to do about it.
- When a fix ships, we will tell you which version carries it.

## Disclosure

Please keep the details private until a fixed version is available, so that
users are not exposed to a problem they cannot yet avoid. If you would like to
be credited in the [CHANGELOG](CHANGELOG.md), say so in your report — we will
not name you unless you ask us to.

There is no bug bounty programme, and we cannot pay for reports.

## Versions that receive fixes

Only the most recent released version. There is no long-term support branch: a
fix ships as a new version of the extension, and updating is how you receive
it.

## In scope

GlyphCove is a VS Code extension that edits Markdown and converts documents,
and it does not communicate over the network. Reports about the extension's own
code are in scope — for example:

- Content of a document that gets past sanitizing and runs as script, in the
  preview or in converted HTML
- A message between the webview and the extension host that passes validation
  when it should not
- Reading or writing a file outside the folders the extension is meant to
  touch, or overwriting a file it was not asked to overwrite
- An argument reaching an external process (PlantUML, a headless browser,
  draw.io) in a way that lets the document decide what gets executed
- **Anything that makes GlyphCove open a network connection**, because it is
  built not to

## Out of scope

- Vulnerabilities in VS Code itself, or in a runtime you installed yourself
  (Java, a browser, another extension) — please report those to their authors.
  But if the way GlyphCove invokes them turns a harmless bug into an
  exploitable one, that **is** in scope and we want to hear about it.
- Documented, intended behaviour: converting a file you pointed the extension
  at, running an external tool you configured, and so on.
  [docs/guide/security.md](docs/guide/security.md) describes how the extension
  is built — behaviour that contradicts that document is worth reporting.
- Output of an automated scanner with no explanation of what an attacker could
  actually do with it.

---

## GlyphCove のセキュリティ報告について

### 脆弱性の報告

GlyphCove に脆弱性と思われるものを見つけた場合は、**公開の Issue にしないでください**。
修正がまだ無いうちに、その手口を誰にでも見える場所へ置くことになります。

[glyphcove-docs リポジトリ](https://github.com/aporo0711/glyphcove-docs)の
GitHub 非公開脆弱性報告（**Security → Report a vulnerability**）をお使いください。
そこに書かれた内容は開発者しか読めません。セキュリティの報告はこの窓口だけで受け付けます。

### 報告に書いていただきたいこと

GlyphCove は利用状況を一切集めていないため、**お使いの環境で何が起きたのかを
こちらで調べる手段がありません。** 分かるのは、報告に書かれたことだけです。

- 何をしたか、その結果として攻撃者に何ができるか
- 再現の手順（できれば**最小のサンプル文書**。添付の前に、機密にあたる内容は取り除いてください）
- GlyphCove の版（拡張機能ビュー → GlyphCove）、VS Code の版（ヘルプ → バージョン情報）、OS
- 関わる外部ツールがあれば、その種類と版（PlantUML を動かす Java、PDF 変換に使うブラウザなど）

### 受け取ったあとのこと

- 最初の返信は **5 営業日以内**を目安にしています。1 人で開発しているため、
  これは目標であって確約ではありません。10 営業日たっても返信が無いときは、
  同じ非公開報告から催促してください
- 再現できたかどうかと、どうするつもりかをお伝えします
- 修正が出たときは、どの版に入ったかをお伝えします

### 公表について

修正版が出るまでは、内容を公にしないでください。まだ避けようのない問題に
利用者をさらすことになるためです。[変更履歴](CHANGELOG.md)にお名前を載せてよければ、
報告にその旨をお書きください。お申し出が無いかぎり、こちらから名前を出すことはありません。

報奨金の制度はありません。報告に対して金銭をお支払いすることはできません。

### 修正が入る版

最新の公開版だけです。長期サポートの枝はありません。修正は拡張機能の新しい版として
出ますので、更新して受け取ってください。

### 対象になるもの

GlyphCove は Markdown を編集し、文書を変換する VS Code 拡張機能で、
ネットワークとの通信をしません。拡張機能自身のコードについての報告が対象です。たとえば:

- 文書の中身がサニタイズを抜けて、プレビューや変換後の HTML でスクリプトとして動く
- Webview と拡張機能ホストの間のメッセージが、通ってはいけないのに検証を通る
- 触るはずのないフォルダのファイルを読み書きする、頼まれていないファイルを上書きする
- 外部プロセス（PlantUML・ヘッドレスブラウザ・draw.io）へ渡る引数を通じて、
  文書の側が「何を実行するか」を選べる
- **GlyphCove にネットワーク接続を開かせるもの**（開かない作りにしているため）

### 対象にならないもの

- VS Code 自身や、ご自分で入れたランタイム（Java・ブラウザ・他の拡張機能）の脆弱性。
  それぞれの作者へお願いします。ただし、**GlyphCove の呼びかたが原因で、
  害の無いはずの不具合が悪用できるものに変わっている場合は対象です**
- 文書化された意図どおりの動き（指定したファイルを変換する、設定した外部ツールを
  起動する、など）。作りは[セキュリティの手引き](docs/guide/security.ja.md)にあります。
  この文書と食い違う挙動を見つけたのであれば、それは報告する価値があります
- 自動検査ツールの出力だけで、攻撃者に実際に何ができるのかの説明が無いもの
