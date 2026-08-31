# Architecture

**日本語**: [architecture.ja.md](architecture.ja.md)

GlyphCove runs entirely on your machine, in the two processes every VS Code
extension gets:

- **The extension host** owns the files and everything that touches disk: opening
  the document, applying the edits the editor sends back, and running every
  conversion (HTML / PDF / Word / Excel, and back). Diagrams (Mermaid / PlantUML)
  and math (KaTeX) are rendered locally as well — document content never leaves
  the machine.
- **The webview editor** renders the preview you edit. When you edit, only the
  blocks you actually touched are re-serialized, so Markdown you did not edit
  stays byte-for-byte unchanged — the product's first design rule.

Every message that crosses the boundary between the two is validated on receipt,
on both sides, and the webview runs under a strict Content-Security-Policy (the
first ADR below records the one deliberate exception). The preview and the
converted output share the same rendering code, so what the editor shows is what
a conversion produces.

## ADR (design decision records)

- **ADR: `'unsafe-inline'` is added to `style-src` only, for Mermaid rendering** (v0.1.2 and later):
  mermaid injects an inline `<style>` both when measuring labels and in the SVG it outputs, and does
  not work under a strict style-src. The alternative (extract the style out of the SVG and move it
  into a stylesheet of our own) was rejected because the font mismatch at measurement time breaks
  the layout. **script-src stays nonce-only**, and `img-src`/`font-src`/`connect-src` are unchanged
  as well. mermaid runs with `securityLevel: 'strict'` + `htmlLabels: false`, and the SVG it outputs
  is **passed through DOMPurify (SVG profile) before** it is inserted into the DOM. The same
  trade-off VS Code's built-in Markdown preview makes by allowing `'unsafe-inline'` in style-src
- **ADR: the PlantUML rendering engine is "reuse the plantuml.jar bundled with the jebbs.plantuml extension"** (v0.1.2 and later):
  PlantUML has no offline JS build that runs inside a Webview, so rendering happens in a Java
  process on the host side (`java -jar plantuml.jar -tsvg -pipe`, spawned with an argument array, no
  shell). Three ways of obtaining the jar were compared — (a) bundle the MIT edition (about 17.6MB
  at v1.2026.6) with the extension, (b) reuse the jar that the PlantUML extension (jebbs.plantuml)
  bundles, (c) let the user name one in the settings — and the decision is **(b) as the base, plus
  (c) `glyphcove.plantuml.jarPath` to override it**. It has been verified on a real machine
  that jebbs.plantuml v2.18.1 bundles `plantuml.jar` (11,863,156 bytes ≈ 11.9MB) directly under the extension root (the
  same precedent as reusing the app bundled with the draw.io extension). (a) was rejected because it
  bloats the VSIX by about +18MB (environments where (b) is unavailable can be covered by (c)).
  **The user chooses which licence edition of the jar to use** (once it is not bundled, the choice
  falls to the user). PlantUML distributes one and the same product under GPLv3 / GPLv2 / LGPL / ASL
  / BSD / EPL / MIT (<https://plantuml.com/en/download>, retrieved 2026-08-17), and the MIT edition
  is placed in the official releases as `plantuml-mit-<version>.jar`. The four entries with no mark
  against the MIT edition in the official table are Ditaa / Jcckit / Sudoku / ELK, so nothing is
  missing from its ability to generate UML diagrams. **The advertising does not go away in the MIT
  edition either** — the sentence "may display a sponsor or advertising message on the welcome /
  error image" appears with the same wording in the `-license` output of both the GPL and the MIT
  edition. This product throws the error image away on a syntax error, so it does not surface on the
  normal path, but **we cannot write "switch to the MIT edition and the advertising goes away"**.
  Which edition the jar on hand is can be told with `java -jar <jar> -license` (the method the
  official FAQ points to). When a path is stated explicitly in the settings, **it is an error if
  that path is not found** (there is no silent fallback to auto-detection). In an environment with
  no Java / jar, nothing is rendered and only a message is shown (honest degradation)
- **ADR: math adopts KaTeX alone. MathJax is not adopted** (v0.1.2 and later):
  The candidate rendering engines were (a) KaTeX, (b) MathJax, (c) a setting that switches between
  the two. **(a) was adopted**. There are three reasons —
  ① **it can render through a synchronous API** (`katex.renderToString`). The preview's NodeView and
  the HTML/PDF conversion can both call **the same single function** (`renderMath` in
  `shared/math.ts`), so this product's basic policy, "the preview and the conversion result never
  disagree", is satisfied **structurally**. MathJax v3 rendering assumes a browser DOM, so using it
  on the host side (Node) would require the same **headless browser + loopback server** as Mermaid,
  which adds one more external browser dependency to PDF conversion.
  ② **it is small** (katex.min.js about 270KB + woff2 about 260KB — a rounding error next to
  mermaid's 3.5MB), so it needs neither a lazy-loading mechanism like Mermaid's nor any addition to
  the CSP.
  ③ what this product handles is business documents, and it does not need the areas where MathJax
  has the advantage (`\require`, custom macro packages, AsciiMath and so on).
  (c) was rejected because "the rendered result changes with a setting = how a document looks
  becomes dependent on the environment" (if it ever becomes necessary, the ADR gets rewritten)
- **ADR: inline math is `$$…$$`. A lone `$…$` is not treated as math** (v0.1.2 and later):
  Measuring remark-math's default (`singleDollarTextMath: true`) showed that it **breaks existing
  documents**, so it was disabled. Measured results:
  - Parser side: `価格は $100〜$200 です。` becomes `inlineMath("100〜")`, and
    `環境変数 $HOME と $PATH` becomes `inlineMath("HOME と ")`
    (two `$` on the same line are enough for it to match)
  - **Serializer side**: `mdast-util-math` registers `$` as a character that needs escaping, so a
    one-character edit to a block that merely mentions a currency amount turns `$100`
    **into `\$100`** (the same class of accident as the GitHub Alerts marker becoming `\[!NOTE]`)

  With `singleDollarTextMath: false` a lone `$` is completely inert on both sides, and currency
  notation round-trips byte-for-byte unchanged. The only thing escaped is a run of two or more `$`
  (`$$` → `\$$`), and that conversion is needed to avoid ambiguity.
  **This value is held in one place, `MATH_OPTIONS` in `shared/math.ts`, and the same thing is
  passed to both the parser and the serializer** (change only one of them and the round trip breaks).
  The price is incompatibility with GitHub's `$…$` notation, and that is stated explicitly in
  [Known normalisations and limitations](guide/limitations.md)
