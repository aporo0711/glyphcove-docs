# 必要ランタイム・外部ツール

**English**: [requirements.md](requirements.md)

| 要件 | バージョン | 用途 |
| --- | --- | --- |
| VS Code | 1.90 以上 | 本体 |
| Microsoft Edge または Google Chrome | インストール済みのもの | **MD→PDF変換**、**HTML/PDF/DOCX/XLSX 変換時のMermaid図のSVG事前変換**、**DOCX/XLSX へ埋め込む図とSVG画像のPNG化**（自動検出、`glyphcove.pdf.browserPath` で指定可。見つからない場合は図をコードブロックのまま出力し警告）。**画像化（スクリーンショット）はChromeが必要です** — この環境のEdge 151はヘッドレスで画像を書き出しません（PDF印刷が無反応なのと同じ症状） |
| Java + plantuml.jar（任意） | Java 8 以上 / PlantUML 拡張機能（jebbs.plantuml）同梱の jar を自動再利用 | **PlantUML図のプレビュー描画と HTML/PDF 変換時のSVG事前変換**（Java は JAVA_HOME/PATH から自動検出。`glyphcove.plantuml.jarPath` / `javaPath` で明示指定可。見つからない場合はプレビューに案内を表示し、変換はコードブロックのまま出力し警告）。**jar は本製品に同梱していません** — どの版を使うかは利用者が選べます（下記「plantuml.jar はどれを使うか」） |
| Node.js | 20 以上 | 開発時のみ |

> PDF変換は、インストール済みのブラウザを順に試し、**実際にPDFを生成できたもの**を使います。
> 一部の環境ではEdgeがヘッドレス印刷に応答しない（正常終了するのに何も出力しない）ため、
> その場合は自動的にChromeへ切り替え、その旨を警告として表示します。
> どのブラウザでも生成できない場合は `glyphcove.pdf.browserPath` で実行ファイルを指定してください。

## plantuml.jar はどれを使うか

> jar は本製品に同梱していないため、描画に使うのは利用者の環境にある jar です。
> 何も設定しなければ **PlantUML 拡張機能（jebbs.plantuml）が同梱する jar** を自動で再利用します
> （v2.18.1 が同梱しているのは PlantUML 1.2024.3 の **GPLv2 版**でした）。
> 設定 `glyphcove.plantuml.jarPath` に別の jar のパスを書けば、そちらが使われます
> （そのパスが見つからなければエラーにし、自動検出へ黙って戻ることはしません）。
>
> **PlantUML 公式は同じ本体を複数のライセンスで配布しており、MIT 版も選べます。**
> [公式のダウンロードページ](https://plantuml.com/en/download)から
> `plantuml-mit-<バージョン>.jar` を入手し、そのパスを `glyphcove.plantuml.jarPath` に
> 設定してください（v1.2026.6 で約 17.6MB）。公式の対応表で MIT 版に印が付いていないのは
> **Ditaa / Jcckit / Sudoku / ELK レイアウト**の 4 つで、公式も「他の版は ditaa 連携などの
> 一部機能を欠くが、UML 図の生成には十分」と説明しています。本製品が使うのは
> シーケンス・クラス・アクティビティ・マインドマップ等の図と smetana レイアウト、
> それに jar 内蔵の標準ライブラリ（`!include <C4/…>`）なので、いずれも影響を受けません。
>
> ⚠ **`plantuml-mit-light-*.jar` は選ばないでください。** 標準ライブラリを含まないため
> `!include <C4/…>` が失敗します。`light` の付かない方を選んでください。
>
> ⚠ **どの版を選んでも、PlantUML は「welcome 画像・エラー画像にスポンサーまたは広告の
> メッセージを表示することがある」と自ら明記しています。** この一文は GPL 版・MIT 版の
> どちらの `java -jar <jar> -license` 出力にも同じ文言で入っており、**MIT 版にすれば
> 消えるというものではありません**。本製品はエラー画像を変換結果に混ぜません —
> 構文エラーは stderr の報告（`-stdrpt`）で、**PlantUML 自身の異常終了は stderr の
> Java スタックトレース**で検出し、どちらの場合も画像を捨ててメッセージに置き換えます。
>
> ⚠ **異常終了は jar の版によって起きます。** 1.2024.3（jebbs.plantuml 2.18.1 同梱）は、
> 中身が `title` だけのクラス図で `java.lang.IllegalArgumentException` を投げます
> （1.2026.6 では解消済み）。異常終了した図は
> 「PlantUML が異常終了しました。jar の版を変えると直ることがあります」と
> 報告されるので、**そのときは新しい版の jar を指定してください**。
> （例外のクラス名が分かるときは括弧で添えます。**この文はエディタの表示言語で出ます** —
> 変換ログと品質レポートも同じです。機械で読むときは文ではなく
> コード `PLANTUML_CRASHED` で読んでください）。
>
> 手元の jar がどの版かは `java -jar <jar のパス> -license` で確認できます
> （公式 FAQ が案内している方法です）。
