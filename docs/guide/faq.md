# FAQ

**日本語**: [faq.ja.md](faq.ja.md)

## Does it really send nothing anywhere?

**Your document itself is never sent anywhere.** There is no cloud API and no telemetry, and the preview and the Word / Excel conversions do not fetch external `http(s)` images. PDF printing drives a browser executable (Edge / Chrome) that is already installed on your machine.

There is one known exception: **PDF conversion** — when a document embeds an external `http(s)` image, the local browser doing the printing fetches it (details and the workaround are in [Known normalisations and limitations](limitations.md); suppressing this is planned).

You do not have to take this on trust — it is checkable. Resource Monitor, TCPView or a proxy log will show you exactly what your machine does. The flip side: **we cannot tell you what you converted**, because nothing about it ever reaches us. Details are in [Security](security.md).

## Edge is installed, but no PDF comes out

Some builds of Edge do not respond to headless printing (they exit normally but write nothing). GlyphCove tries the installed browsers in order and uses **the one that actually produced a PDF**. When Edge does not respond it switches to Chrome automatically and says so in a warning. If no browser works, point the setting `glyphcove.pdf.browserPath` at an executable directly. Details are in [Runtimes and external tools](requirements.md).

## PlantUML diagrams are not rendered

Rendering PlantUML needs Java 8 or later plus a plantuml.jar. **The jar is not bundled** — which licence of PlantUML to run is your choice, not ours. If the PlantUML extension (`jebbs.plantuml`) is installed, its jar is reused automatically and there is nothing to download. How to pick the MIT build, and version-specific pitfalls, are in [Runtimes and external tools](requirements.md). When rendering is not possible the diagram is emitted as a code block, with the reason in a warning.

## The preview renders my Mermaid diagram, but conversion turns it into a code block

Conversion (HTML / PDF / Word / Excel) has to pre-render diagrams to SVG or PNG, and that uses Edge / Chrome (detected automatically). Where no browser is found, the diagram is emitted as a code block and the reason appears in a warning (`DIA002`). Details are in [Runtimes and external tools](requirements.md).

## I pasted a table copied from Excel and merged cells shifted the columns

A known limitation. Tables copied from Excel are pasted as-is (to keep the existing behaviour unchanged), so merged cells are not unmerged and columns shift. Tables from Word and web pages *are* unmerged on paste — the value goes to the top-left cell, with a notification. The workaround is to **unmerge the cells in Excel before copying**. The other limitations are in [Known normalisations and limitations](limitations.md).

## Word says "No table of contents entries found"

Not a bug — it is how Word works. The table of contents GlyphCove writes is a **real TOC field**, so it can be uncalculated when the file is first opened. Select the TOC and press **F9** to build it. Page numbers are only calculated in Print Layout view (Draft and Web Layout do not paginate).

Note that the TOC and figure/table numbering are enabled per conversion via settings (`glyphcove.docx.toc` / `glyphcove.docx.numberCaptions`; **both default to off**).

## What does a conversion lose?

Markdown cannot express everything Word can, and the reverse is just as true. Which directions are lossy is in the "Conversions" table in the README; the specifics are in [Conversion](conversion.md) and [Known normalisations and limitations](limitations.md). The important part: **whatever is dropped is, as a rule, reported in warnings and in a quality report** (things that cannot be detected in principle — figures drawn as shapes inside a PDF, for example — are listed in [Known normalisations and limitations](limitations.md)). A file that quietly lost a column is treated as a defect, not as the price of conversion.

## Will features that are free today become paid later?

Some features are planned to require a paid Pro licence in a future release (Word / Excel output, the reverse conversions, and so on). But the line is already drawn, in full, in the README under "[Pricing plans (advance notice)](https://github.com/aporo0711/glyphcove-docs/blob/main/README.md)" — and **features declared "stays free" (writing, previewing, pasting, and basic HTML / PDF output) will not be moved behind a licence later**. We promise no dates. Until a version says otherwise, everything remains free.

## Can I use it at work, commercially?

Yes. **It is free to use, commercially or non-commercially, with no time limit** (and no limit on machines or people). It is closed-source software, and redistribution is not permitted. The summary is in the README's [License section](https://github.com/aporo0711/glyphcove-docs/blob/main/README.md); the full text that actually governs is the bundled LICENSE.

## How much will Pro cost?

Undecided. When it is decided, the README and the CHANGELOG will say so before anything takes effect (we promise no dates). Which features are planned to be affected is already written in the README under "Pricing plans (advance notice)".

## I found a bug. Where do I report it, and when will I hear back?

The single place is GitHub [Issues](https://github.com/aporo0711/glyphcove-docs/issues). What makes a useful report is in [CONTRIBUTING.md](../../CONTRIBUTING.md) (a minimal reproducing sample helps most — please remove anything confidential first).

GlyphCove is developed by one person. Replies happen on weekday evenings and weekends and can take a few days. Bugs that corrupt documents or output files are looked at first. Every request is read, but implementation cannot be promised.

If you believe you found a vulnerability, please do not open a public issue — use the private reporting described in [SECURITY.md](../../SECURITY.md).
