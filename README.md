# PromptForge ⚡

Client-side AI prompt optimizer — cut tokens, cut cost, cut latency. **No prompts ever leave your browser.**

Open `index.html` in any modern browser. No build step, no dependencies (except Google Fonts).

## What it does

Paste a prompt → PromptForge strips filler, compresses verbose phrasing, normalizes whitespace, and shows you exactly how many tokens (and dollars) you saved against your chosen model.

- 10 toggleable optimization rules (whitespace, politeness fillers, verbose phrases, redundancies, weak qualifiers, list compaction, deduplication, …)
- 3 preset levels: **Light**, **Balanced**, **Aggressive** + **Custom**
- Live cost estimation across **GPT‑4o**, **GPT‑4o mini**, **GPT‑4 Turbo**, **o1**, **o1‑mini**, **Claude 3.5 Sonnet/Haiku**, **Claude 3 Opus**, **Gemini 1.5 Pro/Flash**
- Word-level **diff view** — see exactly what was removed
- **History** of saved optimizations + lifetime savings counter (localStorage)

See [SPEC.md](./SPEC.md) for the full specification, optimization rules, and token-estimation methodology.
