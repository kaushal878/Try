# PromptForge — AI Prompt Optimizer

## Project Overview
- **Project Name**: PromptForge
- **Type**: Single-page, fully client-side webapp (single `index.html`)
- **Core Functionality**: Reduce token usage and processing cost of AI prompts by stripping filler, compressing verbose phrasing, and normalizing whitespace — without changing intent
- **Target Users**: Prompt engineers, AI app developers, anyone calling LLM APIs at scale

## Goals
- Lower **token cost** of prompts sent to LLM providers (OpenAI, Anthropic, Google, etc.)
- Lower **latency** by reducing input tokens that the model has to process
- Preserve prompt intent — never rewrite meaning, only remove low-value words
- Be **100% client-side**: no prompts ever leave the user's browser

## UI/UX Specification

### Layout
- **Sticky header** with brand and lifetime savings counters
- **Hero** with the value proposition
- **Stats bar** (5 cards): input tokens, optimized tokens, % saved, est. $ saved, est. ms saved
- **Toolbar**: Level selector (Light / Balanced / Aggressive / Custom), Model + runs selector, Sample loader, Optimize button
- **Two-column editor grid**: Original prompt (textarea) on the left, Optimized prompt on the right (with Show diff toggle)
- **Rules grid**: Toggle individual optimization rules. Manual toggling switches level to "Custom"
- **Bottom grid**: Tips card + History card (last 20 saved optimizations)
- **Toast** for feedback (copied, saved, etc.)

### Visual Design
- Dark, developer-tool aesthetic — deep navy background, cyan/violet gradients
- Monospace (JetBrains Mono) for editor and metrics, Inter for UI, Space Grotesk for headings
- Gradient brand logo + sticky header with backdrop blur

### Color Palette
- Background: `#07090f`, surfaces `#0c1020` → `#1a2240`
- Primary accent (Cyan): `#22d3ee`
- Secondary accent (Violet): `#a78bfa`
- Tertiary (Pink): `#f472b6`
- Success (Green): `#34d399`, Warning (Amber): `#fbbf24`, Danger (Red): `#f87171`

## Functionality Specification

### Token Estimation
Heuristic that combines two well-known approximations:
- `chars / 4` — OpenAI's published rule for English
- `words × 1.33 + specials × 0.4 + newlines × 0.3` — closer for short words and code

The maximum of the two is used (more conservative for cost calculations). Typically within ±5% of `tiktoken.cl100k_base` for English prose.

### Optimization Rules
Each rule is independently toggleable. Levels are presets that select a subset.

| Rule | Description | Light | Balanced | Aggressive |
|---|---|:---:|:---:|:---:|
| Collapse whitespace | Multiple spaces/blank lines collapsed | ✓ | ✓ | ✓ |
| Normalize punctuation | Smart quotes → straight, `!!!` → `!` | ✓ | ✓ | ✓ |
| Compress verbose phrases | "in order to" → "to" (~40 patterns) | ✓ | ✓ | ✓ |
| Strip politeness fillers | "please", "kindly", "could you" |  | ✓ | ✓ |
| Remove redundant phrases | "as you know", "needless to say" |  | ✓ | ✓ |
| Strip greetings & throat-clearing | "Hi there!", "I hope this finds you well" |  | ✓ | ✓ |
| Drop weak qualifiers | "very", "really", "actually", "basically" |  |  | ✓ |
| Compact short lists | Inline `- a / - b / - c` → `a; b; c` |  |  | ✓ |
| Deduplicate sentences | Drop repeated sentences |  |  | ✓ |
| Trim filler articles (risky) | Drop "the/a" before plural nouns | (off) | (off) | (off) |

### Live Behavior
- Optimization runs on input (debounced ~80ms) and on every toggle/level/model change
- Output panel renders either plain optimized text or **inline word-level diff** (LCS) with red strikethrough for removed words and green highlight for added/changed
- Stats update in real time (input tokens, output tokens, % saved, $ saved, ms saved)

### Pricing
Per-1M-token input rates for major models (early 2026 published rates):
GPT-4o, GPT-4o mini, GPT-4 Turbo, o1, o1-mini, Claude 3.5 Sonnet/Haiku, Claude 3 Opus, Gemini 1.5 Pro/Flash.

Cost saved = `(saved_tokens / 1,000,000) × input_rate × runs`.

Latency saved estimate = `saved_tokens × 0.6 ms/token`.

### Persistence (localStorage)
- `pf_history` — last 20 saved optimizations
- `pf_lifetime` — total tokens saved across all sessions
- `pf_lifetimeCost` — total $ saved across all sessions

### User Interactions
- Type/paste prompt → live optimization
- Click any rule card → toggle that rule (level switches to "Custom")
- Click level segment → apply preset
- Click sample → load a verbose example prompt
- **Copy** / **Download** / **Save** buttons on output panel
- **Show diff** toggle highlights removed/added words inline
- Click history item → reload that prompt into input

## Acceptance Criteria
- [x] Single-file `index.html`, no build step, opens directly in browser
- [x] Optimization is purely client-side (no network calls except Google Fonts)
- [x] At least 8 toggleable optimization rules
- [x] 3 level presets (Light, Balanced, Aggressive) + Custom
- [x] Live token estimation for input and output
- [x] Cost estimation across at least 8 different models
- [x] Side-by-side editor with word-level diff view
- [x] Copy, download, save, paste, clear actions all work
- [x] History persists in localStorage across reloads
- [x] Lifetime savings counter persists across reloads
- [x] Responsive on mobile, tablet, desktop
