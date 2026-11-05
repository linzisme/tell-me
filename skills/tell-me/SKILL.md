---
name: tell-me
description: Explain a requirement, mechanism, change, proposal, or problem as a narrative stack of cards, rendered inline in the conversation or as an HTML page.
disable-model-invocation: true
---

Explain the topic to the user as one stack of cards they read top to bottom and come away understanding. The topic is `$ARGUMENTS`; when empty, use the current topic of conversation. When the topic comes with source material (a doc, diff, PR, or code), read it first. Write the page in the user's language.

Start from the one sentence the reader should remember, then order the page by what is being explained:

- Requirement: pain point → goal → user scenarios → in / out of scope → key rules → acceptance → open questions
- Mechanism: analogy → core idea → step by step → limits and parameters → common misconceptions → how to use it
- Change: what changed → why → before / after → blast radius → risk and rollback → how to verify
- Proposal: problem and constraints → options → comparison → chosen design → risks → pending decisions
- Problem: the contradiction → expectation → reality → causes → the loop → fixes → the reframe

The order is a starting point; add, drop, or merge sections to fit the content.

Pick each section's component by the shape of its information:

- Thesis or contradiction → highlighted callout at the top
- Process or call chain → horizontal icon → icon flow
- Parallel items → a row of two or three small cards
- Bottleneck or focal point → a single card with an orange border
- Causes or key points → a card list, each with icon, title, and one-line note
- Before / after, old way / new way → two columns, old struck through, new checked
- In / out of scope, option comparison → two columns or a small table
- Feedback loop or causal chain → a vertical arrow chain, last link in red
- Steps or fixes → a numbered list
- Trends or metrics → the number with ↗ / ↘
- Punchline → a red or green emphasis box
- Code shape, file tree, sequence → a monospace block or inline SVG diagram

Number section headings `01 / Title`, the number muted, and close with an "in one sentence" box in the positive color. Keep the cards neutral and spend color as an accent: icons, key numbers, a card's border or label. Color carries meaning: green for goals and positives, orange for focus and bottlenecks, red for negatives and hard-hitting conclusions. Reserve tinted backgrounds for the two or three boxes that carry the argument (the thesis, the punchline, the closing sentence), with body text in the primary text color even there. Let HTML and CSS carry the text and layout; use SVG for icons and for diagrams such as loops, arrows, and architecture.

Render inline when the host offers an inline HTML widget (in the Claude app, `show_widget`): the whole explanation goes in the widget, headings and prose included, since the cards are the answer. Use the host's CSS variables (`--bg-success`, `--text-danger`, …) and its icon font, so the cards follow the light or dark theme.

Without an inline widget, write a page to `/tmp/tell-me-<topic>.html` and `open` it. Give it a dark palette: background `#0d1117`; cards `#161b22` with a 1px `#30363d` border and 12px radius; body text light grey, headings white; green `#3fb950`, orange `#d29922`, red `#f85149`; `color-scheme: dark` on `:root`. Put the content in a `<main>` reading column centered on the page, with multi-column groups reflowing on narrow screens.

When the user wants an image, write the page file as above, then export `<main>` as a PNG:

```bash
python3 - /tmp/tell-me-<topic>.html /tmp/tell-me-<topic>.png <<'EOF'
import sys
from playwright.sync_api import sync_playwright
with sync_playwright() as p:
    b = p.chromium.launch(channel="chrome")
    g = b.new_page(viewport={"width": 1280, "height": 800}, device_scale_factor=2)
    g.goto("file://" + sys.argv[1])
    g.locator("main").screenshot(path=sys.argv[2])
EOF
```
