# GlyphCove for VS Code

Convert Markdown to and from Word, Excel, PDF, and HTML, and edit it non-destructively in the preview.

> **Use of this extension is governed by the GlyphCove License Agreement (version 1.2), included as [LICENSE](LICENSE).**
> Installing, copying, or using it is deemed acceptance of that agreement.
> The agreement is written in Japanese and **the Japanese text is the authoritative version**; the [License](#license) section below is a convenience summary and is not part of the agreement.

**日本語**: [README.ja.md](docs/README.ja.md) ／ Changelog: [CHANGELOG.md](CHANGELOG.md) ／ Feedback: [CONTRIBUTING.md](CONTRIBUTING.md)

![Pasting an Excel selection into the GlyphCove preview](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/gifa-en.gif)

*A table copied from Excel becomes a Markdown table on paste.*

## Who it is for

You write specifications, minutes and reports in Markdown — and the people you hand them to expect Word, Excel or PDF.

**You keep writing Markdown. What you hand over is still Word.**

- **You do not have to stop using Word, and you do not have to convert what you already have.** Only the writing changes; the submission does not. Starting with the next document you write is enough.
- **Your file stays your file.** Blocks you did not edit are not rewritten by a single byte — not the fence characters you chose, not the line you wrapped by hand — so your Git diff shows what you changed and nothing else. Pasted images are written next to the document as files; no base64 is ever generated.
- **Nothing leaves your machine.** No cloud API, no telemetry, no per-page fees. The preview and the Word / Excel conversions do not fetch external `http(s)` images. Put plainly: **we cannot tell you what you converted, because nothing about it ever reaches us.**
- **What a conversion loses is reported, not hidden.** Markdown cannot express everything Word can, and the reverse is just as true. Where something is dropped or flattened, GlyphCove says so in a warning and in a quality report. **A file that quietly lost a column is treated here as a defect, not as the price of conversion.**
- **You do not have to learn the markup to be handed a document.** A table pasted from Excel, Word, a web page or box-drawn console output arrives as a table; headings, lists and diagrams are edited in the preview itself.

If what you need is a Markdown editor, VS Code already has good ones. This is for the part that comes after: getting the document out in the shape somebody else asked for.

## What it does

- **You edit in the preview itself.** Write without thinking about the markup, and **blocks you did not edit are not rewritten by a single byte** (your Git diff shows only what you actually changed).
- **Diagrams and math render in place.** Mermaid, PlantUML, and KaTeX render in the preview; click one and its source opens and updates live. `*.drawio.svg` diagrams open in draw.io on double-click.
- **Tables paste as tables.** From Excel, Word, a web page, box-drawn text, or CSV / TSV.
- **Images stay as files.** Pasted images are written to `<document-name>_images/` and referenced by path — no base64 is ever generated.
- **Paste into Word or PowerPoint with formatting.** The `Office` button on the toolbar copies both rich formatting and the original Markdown.
- **Broken links are reported in the Problems panel.** Missing relative links, missing images, and `#heading` anchors that do not exist in the open document.

![The GlyphCove preview with a Markdown document being edited](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/sp1-en.png)

*Editing Markdown in the preview.*

![Pasting box-drawn terminal output into the GlyphCove preview](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/gifb-en.gif)

*Box-drawn output shown in a terminal becomes a table on paste, too.*

## What it does not do

- **External `http(s)` images are not loaded by the preview or the Word / Excel conversions** — opening a document must never cause network traffic, so this is by design. One known exception: PDF printing runs a local browser, and if a document embeds an external image that browser fetches it ([details and workaround](docs/guide/limitations.md); suppressing this is planned).
- **Scanned PDFs are not OCRed**, and PDF headers/footers are not extracted (both are stated in warnings, not silently skipped).
- **Tables cannot be pasted from a PDF** — the clipboard carries no table structure. Use PDF → Markdown conversion instead; it reads the drawing commands.
- **Merged cells pasted from Excel shift columns** (known limitation — Excel tables are pasted unchanged; tables from Word and web pages are unmerged on paste, with a notification).
- **Math is `$$…$$` only**, and in Word output formulas come out as TeX source in monospace text, not rendered equations.
- **No collaborative editing, no cloud sync, no mobile app.**

The full lists, with the reasons: [Known normalisations and limitations](docs/guide/limitations.md) and [What we do not plan to do](docs/guide/not-planned.md).

## Conversions

![A diagram of the supported conversions between formats](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/s3-matrix-en.png)

*What converts to what.*

| Conversion | Lossy |
| --- | --- |
| Markdown → HTML | no |
| Markdown → PDF | no |
| Markdown → Word (DOCX) | yes |
| Markdown tables → Excel (XLSX) | yes (what is not a table goes to separate sheets) |
| PDF → Markdown | yes |
| Word (DOCX) → Markdown | yes |
| Excel (XLSX) → Markdown | yes |
| HTML → Markdown | yes |
| Word (DOCX) → HTML | yes |
| Excel (XLSX) → HTML | yes |

Several `.md` files can be merged into a single PDF / HTML / Word / Excel file. **Where a conversion loses something, GlyphCove reports what was lost** in warnings, rather than pretending it succeeded; the reverse conversions (PDF / Word / Excel / HTML → Markdown) also produce a quality report.

![A Markdown document converted to a Word file, opened in Word](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/gifc-en.gif)

*Markdown converted to Word. The table of contents is a real field — Ctrl+click jumps to the heading.*
**The converted `.docx` is opened in Word here to show it.**

![A conversion quality report listing what was not carried over](https://raw.githubusercontent.com/aporo0711/glyphcove-docs/main/docs/images/store/s2-report-en.png)

*The quality report names what a conversion could not carry over.*

## Interface language and output language

- **The interface follows your VS Code display language** — English and Japanese are supported.
- **What GlyphCove writes into a converted file follows the language of the document itself**, not your display language. An English Markdown file converts to a Word document with a `Contents` heading, `Note` / `Tip` / `Important` / `Warning` / `Caution` callout titles, `Figure 1` / `Table 1` captions and a `CONFIDENTIAL` watermark; a Japanese one gets `目次`, `ノート`, `図 1`, `社外秘`. Generated HTML declares the matching `lang`.
- The language is read from the document: front matter `lang:` (or `language:` / `言語:`) if it is there, otherwise from the writing in the body. **Both are decided by the file alone, so the same input always produces the same output** — a converted document does not change because a different person ran the conversion. Only a document with no language in it at all (a bare table of numbers) falls back to your display language.
- **Messages about a conversion follow your display language** — the conversion log (`.log.txt`), the quality reports and the warning text. They describe the run, not the document.

## Fully offline

- **Nothing is ever sent to a cloud API.** PDF printing drives a browser executable already installed on your machine.
- **External `http(s)` images are not loaded by the preview or the Word / Excel conversions** — opening a document never causes network traffic. The one known exception is PDF printing ([details](docs/guide/limitations.md)).
- In an untrusted workspace, nothing that starts an external process runs: no conversion, and no diagram rendering in the preview or in "Copy for Office" (Workspace Trust).

## Requirements

| Requirement | Version | Used for |
| --- | --- | --- |
| VS Code | 1.90 or later | the extension itself |
| Microsoft Edge or Google Chrome | any installed build | PDF conversion, and rendering diagrams to SVG and PNG (detected automatically) |
| Java + plantuml.jar (optional) | Java 8 or later | Rendering PlantUML diagrams. **The jar is not bundled** — you choose which build to use, including the MIT-licensed one ([download page](https://plantuml.com/en/download); if the PlantUML extension (`jebbs.plantuml`) is installed, its jar is reused automatically and there is nothing to download). Without it, GlyphCove says so and leaves the diagram as a code block. |

## Getting started

- Right-click a `.md` file → **Open in preview editor**
- Right-click a file → **Convert files (Markdown / PDF / Word / Excel / HTML)** to convert to HTML / PDF / Word / Excel
- **While editing in the preview, right-click the editor tab → Convert files** — the document you have open is the one that gets converted
- Right-click a folder → **Batch convert files in a folder**

## Install

There are three ways to install GlyphCove. Use whichever one your editor reads.

**From the Extensions view (VS Code)**

Search the Extensions view (Ctrl+Shift+X) for `GlyphCove` and select **Install**, or from the command line:

```powershell
code --install-extension glyphcove.glyphcove
```

**From Open VSX (Cursor, VSCodium and other editors)**

These editors read the Open VSX registry rather than the Visual Studio Marketplace. Search their Extensions view for `GlyphCove` and install it the same way.

**From a downloaded `.vsix`** — for machines with no access to either store

1. Download the latest `glyphcove-vX.Y.Z.vsix` from the [releases page](https://github.com/aporo0711/glyphcove-docs/releases)
2. Open the Extensions view, then the `…` menu at the top right → **Install from VSIX...**
3. Select the `.vsix` file you downloaded

From the command line:

```powershell
code --install-extension glyphcove-vX.Y.Z.vsix
```

> **About updates**: installs from the Extensions view and from Open VSX update themselves while VS Code's `extensions.autoUpdate` is on (it is by default). **An extension installed from a `.vsix` does not** — when a new release is announced, install the new `.vsix` the same way (it replaces the old one).

Please report bugs and feature requests on [Issues](https://github.com/aporo0711/glyphcove-docs/issues) (see [CONTRIBUTING.md](CONTRIBUTING.md) for what helps).

## Learn more

- [FAQ](docs/guide/faq.md)
- [Preview editing (WYSIWYG)](docs/guide/editing.md)
- [Conversion](docs/guide/conversion.md)
- [Runtimes and external tools](docs/guide/requirements.md)
- [Security](docs/guide/security.md)
- [Known normalisations and limitations](docs/guide/limitations.md)
- [What we do not plan to do](docs/guide/not-planned.md)
- For developers: [Architecture and ADRs](docs/architecture.md)
- [Bug reports and feature requests](CONTRIBUTING.md)
- [Reporting a security vulnerability](SECURITY.md)

## Pricing plans (advance notice)

**Every feature is free to use in the current version.** So that a later change never comes as a surprise, here is the line we plan to draw in a future release:

- **Stays free**: writing, previewing and pasting Markdown, and the basic HTML / PDF output.
- **Planned to require a paid Pro license (one-time purchase)**: output to Word / Excel (MD → DOCX / XLSX), the reverse conversions (PDF / Word / Excel / HTML → Markdown) and the two-stage pairs built on them, batch conversion and merging, quality reports, "Copy for Office", print headers/footers with page numbers, and watermarks.
- **Planned for Enterprise (for organisations)**: policy enforcement and procurement paperwork.

License validation will be **fully offline** (no network traffic), in keeping with the product's zero-transmission design. Until a version says otherwise, everything above remains free. **The "stays free" list will not shrink in a later release.**

## License

Use of this extension is governed by the **[GlyphCove License Agreement](LICENSE)** (version 1.2). The full text ships with the extension as [LICENSE](LICENSE), and you can open it at any time from the command palette with **GlyphCove: Show license agreement**. **The agreement is written in Japanese, and the Japanese text is the authoritative version.**

The points below are a summary for convenience (**this summary is not part of the agreement**; only the full text of LICENSE has effect).

- **Free to use, commercially or non-commercially, with no time limit** (Article 4). There is no limit on the number of machines or people.
- **Everything you create is yours.** We claim no rights over the documents you edit or convert, and that does not change when the agreement ends (Article 3.3, Article 20.3).
- **There are only three distribution channels** — Visual Studio Marketplace, Open VSX, and a direct `.vsix` download from the distribution page (Article 3.4). Copying for installation and backup is explicitly permitted (Article 3.5), but **redistribution is not** (Article 9.1).
- **Because it is fully offline, a version you already have keeps working** even after we stop providing or supporting it (Article 17.4).
- **This software reads, modifies, and creates your files.** Conversion is inherently lossy. **Back up your files before using it, and check the results** (Article 12).
- Limitation of liability is in Article 15. **The cap does not apply to our wilful misconduct or gross negligence.**

> ⚠ **This agreement has not yet been reviewed by a lawyer.** A review is planned before the paid edition ships. The version and effective date are stated at the top of LICENSE.

Licence notices for bundled open-source dependencies are **generated at package time** as `THIRD_PARTY_LICENSES.txt` and shipped inside the `.vsix` (`npm run licenses` / `vscode:prepublish`). They are not kept in the repository.
