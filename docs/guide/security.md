# Security

**日本語**: [security.ja.md](security.ja.md)

- Webview CSP: `default-src 'none'` plus nonce-bearing scripts only. **`unsafe-inline` / `unsafe-eval` are not used in script-src**
- Mermaid itself (mermaid.min.js, 3.5MB) is bundled with the extension and **is not loaded until it is needed**
  (a nonce-bearing `<script>` is injected at the first Mermaid block. No CDN, fully offline)
- HTML that comes from Markdown: the editor sanitizes with micromark + DOMPurify, HTML conversion with rehype-raw + rehype-sanitize (tests confirm removal of script / event attributes / `javascript:`)
- Every message between webview and host is schema-validated for type, length and value range (in both directions)
- External processes are spawned as an executable plus an argument array (no shell string concatenation). Formula-injection countermeasures (Excel output turns cells beginning with `=` and the like into strings)
- `localResourceRoots` (the local resources the webview may read) is kept at the "minimum necessary" of §7.1 of the spec:
  in addition to the extension's `dist/webview`, **only "the folder of the open document" and
  "the workspace folder that contains that document" are allowed, and only so that images can be displayed**
  (since v0.1.2. The home folder and the like are not included).
  `http(s)` is not included in the CSP `img-src`, so external images are never loaded
- Automatic stencil rendering (only when the Draw.io Integration extension and Edge/Chrome are present):
  the draw.io that this extension bundles is used for rendering, isolated on the host side with a
  **headless browser + loopback HTTP server** (as with PDF conversion, no browser is bundled; an
  installed one is launched with an argument array). The `unsafe-inline`/`unsafe-eval` that draw.io
  requires stay contained inside that loopback page, and **the editor webview's CSP and localResourceRoots
  are not relaxed at all**. The server binds to a temporary port on 127.0.0.1 with a **path carrying a
  random token**, and all it serves is the draw.io webapp (read access only) and the two driver pages
  (none of the document's content is ever served). Overwriting a file with the rendered result is done
  **only for a placeholder that a paste created in this session** (the host tracks the URL and
  re-checks the marker in the file content first)
- Mermaid SVG pre-conversion (during HTML/PDF conversion) uses the same isolation:
  all the loopback HTTP server (127.0.0.1, path carrying a random token) serves is the bundled
  mermaid.min.js / purify.min.js, the generated page and the diagram source, and the headless
  browser is thrown away after each batch. The page's own CSP forbids inline scripts,
  eval and external communication, and the SVG is **put through DOMPurify inside the page** before it is collected and verified
- KaTeX runs with **`trust: false`** (the default): `\href` / `\url` / `\includegraphics` are not
  interpreted, so no raw HTML, links or external references are generated from a formula (pinned by a unit test).
  KaTeX's contract is that every string it outputs is already escaped, but a formula is input that
  comes from the document, so **anything that enters the preview is put through DOMPurify as well**
  (the same final gate as mermaid / PlantUML). `maxSize` / `maxExpand` also keep macro expansion and
  size specifications from running away. **The CSP is unchanged**:
  KaTeX emits inline styles, but `style-src` already has `'unsafe-inline'` for Mermaid,
  `font-src` for fonts is already `cspSource`, and `script-src` stays nonce-only
- The PlantUML process is started with **`PLANTUML_SECURITY_PROFILE=SANDBOX`**:
  under the default profile we confirmed in a live environment that `!include C:/…` **can pull the
  contents of a local file into the diagram**, so SANDBOX, which blocks file and network access, was made mandatory
  (the standard library built into the jar, `!include <C4/…>` and so on, has been confirmed to work under SANDBOX too.
  `javascript:` hyperlinks have also been confirmed to be removed by PlantUML itself). In addition, the output SVG is
  shape-validated on the host side (`<script>` and `javascript:` hrefs are rejected), and anything that enters the preview
  is put through **DOMPurify (SVG profile)** on the webview side as well (the same final gate as mermaid).
  The resident process communicates over stdin/stdout pipes only, and **never opens the network**.
  PlantUML outputs an "error image" for a syntax error, but **we detect it through the stderr report (-stdrpt)
  and throw the error image away**, showing it as a message with a line number instead (an error is never embedded as a success).
  ⚠ **`-stdrpt` reports syntax errors only** — if PlantUML itself terminates abnormally, no report
  is emitted and the error image comes back with exit code 0 (measured). For that reason
  **the Java stack trace on stderr is treated as evidence of failure too**. The error image carries
  PlantUML's own guidance text and **a QR code with the diagram source embedded in it**, so
  embedding it as a success would let the document's content out as an image.
  ⚠ Exception **messages are not recorded** (they could quote the document's content). Only the class name is handled
- **The page that turns a diagram's SVG into PNG loads that SVG as an `<img>`** (it never enters the page's DOM):
  a `.svg` file written in the document is read without going through the sanitizer (so that
  `foreignObject` labels are not dropped), which means **embedding it directly in the page would give the document the page's DOM**.
  ⚠ **`default-src 'none'` alone is not enough** — the CSP **has no directive that stops navigation**, so
  a `<meta http-equiv="refresh">` inside a `foreignObject` sails straight through and connects to the outside (measured).
  An SVG placed in an `<img>` has no scripts, no external references and no navigation (blocking confirmed by measurement under the same conditions).
  **What holds "fully offline, zero transmission" is this way of loading, not the CSP**
