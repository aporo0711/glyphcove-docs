# What we do not plan to do

**日本語**: [not-planned.ja.md](not-planned.ja.md)

Finding out after installation that a tool is not what you expected is the most expensive outcome for both of us. So here is the list of things we **do not plan to implement**. Requests are always welcome (every one is read), but the following are not on the roadmap as a matter of policy. If the policy ever changes, the CHANGELOG will say so before the implementation ships.

1. **Telemetry or usage collection** — "your documents never leave your machine" rests on this. This policy will not change
2. **Cloud-API conversion or an online service edition** — every conversion runs entirely on your machine
3. **Fetching external `http(s)` images** — opening a document must not cause network traffic (⚠ one known exception today: PDF conversion fetches external images embedded in the document; suppression is planned — [details](limitations.md))
4. **Accounts or sign-in** — nothing, including licence validation, will assume a network
5. **Collaborative editing / realtime sync** — other tools own that space. GlyphCove owns writing alone and handing over in the format someone asked for
6. **OCR for scanned PDFs** — PDF → Markdown reads the text layer and drawing commands only. OCR quietly shifts the cost of verifying misrecognitions onto you, so it stays out
7. **Markdown dialects beyond GFM + YAML front matter** — more dialects means the same file looks different in different places
8. **Bundling plantuml.jar** — which licence of PlantUML to run is your choice ([how to choose](requirements.md))
9. **Per-update notifications or popups** — we do not interrupt your work. Changes are announced in the CHANGELOG and Releases
10. **Removing features from the free version later** — features declared "stays free" ([pricing plans](../../README.md)) will not be moved behind a licence afterwards

Questions or requests about items on this list may be answered with a link to this page (thank you for understanding). Something that *should* work but does not is a bug, not a non-goal — please check the [FAQ](faq.md) and [Known normalisations and limitations](limitations.md), then head to [Issues](https://github.com/aporo0711/glyphcove-docs/issues).
