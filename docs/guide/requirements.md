# Required runtimes and external tools

**日本語**: [requirements.ja.md](requirements.ja.md)

| Requirement | Version | Purpose |
| --- | --- | --- |
| VS Code | 1.90 or later | The host application |
| Microsoft Edge or Google Chrome | Whichever one is installed | **MD→PDF conversion**, **pre-converting Mermaid diagrams to SVG for HTML/PDF/DOCX/XLSX conversion**, **rasterizing diagrams and SVG images to PNG for embedding into DOCX/XLSX** (auto-detected; can be set with `glyphcove.pdf.browserPath`. If none is found, diagrams are output as code blocks and a warning is recorded). **Rasterizing to an image (screenshot) requires Chrome** — Edge 151 in this environment does not write out images in headless mode (the same symptom as PDF printing not responding) |
| Java + plantuml.jar (optional) | Java 8 or later / the jar bundled with the PlantUML extension (jebbs.plantuml) is reused automatically | **Drawing PlantUML diagrams in the preview, and pre-converting them to SVG for HTML/PDF conversion** (Java is auto-detected from JAVA_HOME and PATH. It can be set explicitly with `glyphcove.plantuml.jarPath` / `javaPath`. If none is found, the preview shows guidance and conversion outputs the code block as-is and records a warning). **The jar is not bundled with this product** — you choose which version to use (see "Which plantuml.jar to use" below) |
| Node.js | 20 or later | Development only |

> PDF conversion tries the installed browsers in order and uses **the one that actually produced a PDF**.
> On some machines Edge does not respond to headless printing (it exits successfully but outputs nothing),
> so in that case it switches to Chrome automatically and reports that switch as a warning.
> If no browser can produce one, set the executable with `glyphcove.pdf.browserPath`.

## Which plantuml.jar to use

> The jar is not bundled with this product, so what draws the diagrams is a jar on the user's machine.
> With no configuration, it automatically reuses **the jar bundled with the PlantUML extension (jebbs.plantuml)**
> (what v2.18.1 bundled was the **GPLv2 build** of PlantUML 1.2024.3).
> If you write the path of a different jar into the `glyphcove.plantuml.jarPath` setting, that one is used
> (if that path is not found it is an error; it does not silently fall back to auto-detection).
>
> **PlantUML upstream distributes the same engine under several licenses, and the MIT build is one you can choose.**
> Get `plantuml-mit-<version>.jar` from the [official download page](https://plantuml.com/en/download)
> and set that path in `glyphcove.plantuml.jarPath`
> (about 17.6MB at v1.2026.6). The items the official table does not tick for the MIT build are the four
> **Ditaa / Jcckit / Sudoku / ELK layout**, and upstream itself explains that "the other builds lack
> some features such as the ditaa integration, but are enough to generate UML diagrams". What this product uses is
> sequence, class, activity, mind map and similar diagrams, the smetana layout,
> and the standard library built into the jar (`!include <C4/…>`), so none of them are affected.
>
> ⚠ **Do not pick `plantuml-mit-light-*.jar`.** It does not contain the standard library, so
> `!include <C4/…>` fails. Pick the one without `light`.
>
> ⚠ **Whichever build you pick, PlantUML states itself that it "may display sponsor or advertising
> messages on the welcome image and on error images".** That sentence is in the `java -jar <jar> -license`
> output of both the GPL build and the MIT build with the same wording, and **it is not something that
> goes away by switching to the MIT build**. This product does not mix error images into conversion results —
> syntax errors are detected through the stderr report (`-stdrpt`), and **a crash of PlantUML itself through the
> Java stack trace on stderr**; in both cases the image is discarded and replaced with a message.
>
> ⚠ **Crashes happen depending on the jar version.** 1.2024.3 (bundled with jebbs.plantuml 2.18.1) throws
> `java.lang.IllegalArgumentException` on a class diagram whose body is nothing but `title`
> (fixed in 1.2026.6). A crashed diagram is reported as
> `PlantUML exited abnormally. Changing the jar version sometimes fixes this.`, so
> **specify a newer jar version when that happens**. (The exception class name is added in
> parentheses when it is known. **This sentence follows the editor display language** — so do the
> conversion log and the quality report. Read the code `PLANTUML_CRASHED` rather than the sentence
> when you need to match on it from a script.)
>
> You can check which version the jar you have is with `java -jar <jar-path> -license`
> (this is the method the official FAQ points to).
