---
name: testing-promptforge
description: Test PromptForge (and other single-file static apps in this repo) end-to-end. Use when verifying the prompt optimizer UI in `index.html` on branch `n1` (or any future branch that ships a self-contained static webapp here).
---

# Testing PromptForge / single-file static apps

This repo (`kaushal878/Try`) ships several self-contained static webapps as a single `index.html` file with embedded CSS/JS and no build step (PromptForge on `n1`, the joke app on `joke111`, the student portal on `AI-student-portal`). They share the same testing pattern.

## Run the app locally

From the repo root:

```bash
python3 -m http.server 8765
```

Open `http://localhost:8765/index.html` in Chrome. No build, no install.

## Before recording

1. Maximize the browser window:
   ```bash
   sudo apt-get install -y wmctrl 2>/dev/null; wmctrl -r :ACTIVE: -b add,maximized_vert,maximized_horz
   ```
2. **Clear `localStorage`** before recording so any counter / history / state assertions start from a known baseline. In the devtools console:
   ```js
   localStorage.clear();
   ```
   then F5. PromptForge persists three keys: `pf_history`, `pf_lifetime`, `pf_lifetimeCost`. Other apps in this repo use similar `<prefix>_*` keys (e.g. joke app uses `nepal_jokes_*`).

## PromptForge — reference values for the default sample

The default-loaded sample (#0, "Verbose customer-service reply") is **deterministic** because the token estimator is a pure function. Use these exact values as pass/fail thresholds — any drift means a rule pipeline regressed:

| Level | Output tokens | Saved % | Tokens removed |
|---|---|---|---|
| Light | ~150 | ≤ Balanced | small |
| **Balanced (default)** | **111** | **27%** | **42** |
| **Aggressive** | **108** | **29%** | **45** |

**Marker words to assert on:**
- After Balanced: output must NOT contain `Hi there!`, `please kindly`, `In order to facilitate`, `Thank you so much`. Output MUST contain `I am writing to you today` and `To help` (compression of `In order to facilitate`).
- Aggressive vs Balanced: Aggressive removes `quite` and `really` (these only disappear at Aggressive because they're handled by the qualifier rule).
- Toggling "Drop weak qualifiers" off while on Aggressive: level auto-switches to **Custom**, token count goes 108 → 111, and `quite`/`really` reappear.

## UI element map (devinids on the default-loaded page)

- Level segment: Light=1, Balanced=2, Aggressive=3, Custom=4
- Model select=5, Runs select=6
- Load sample=7, Optimize=8
- Original textarea=9, Paste=10, Clear=11
- Show/Hide diff=12, Copy=13, Download=14, Save=15
- Rule cards (in DOM order on the page):
  - 16 Collapse whitespace, 17 Normalize punctuation, 18 Compress verbose phrases, 19 Strip politeness fillers
  - 20 Remove redundant phrases, 21 Strip greetings & throat-clearing, 22 Drop weak qualifiers, 23 Compact short lists
  - 24 Deduplicate sentences, 25 Trim filler articles (off by default)
- Clear history=26, history list item=27

**Pitfall — choose the right rule when proving Custom-switch behavior:** The default sample contains no "redundant phrases" (no `as you know`, `needless to say`, etc.) so toggling rule 20 will switch the level to Custom but the token count will stay at 108. To prove the *count* changes, toggle a rule whose patterns actually appear in the sample — `Drop weak qualifiers` (devinid=22) is the safe choice because `quite` and `really` are both in the default sample.

## Verifying the diff view

The diff renders into `#output` as `<span class="removed">…</span>` (red strikethrough) interleaved with `<span class="added">…</span>` (green) and plain kept words. To prove it programmatically:

```js
const out = document.getElementById('output');
const removed = out.querySelectorAll('.removed');
console.log(removed.length, Array.from(removed).slice(0, 5).map(e => e.textContent));
```

…but a screenshot of the rendered red strikethrough is usually enough evidence on its own. The button label flips between `🔀 Show diff` and `📄 Hide diff`.

## CI on this repo

The repo has **no GitHub Actions configured.** The only checks that show up on PRs are third-party Continuous AI bots (`Accessibility Fix Agent`, `News1`). They sometimes error on their own service ("Agent encountered an error") — those failures are NOT code-related, NOT actual CI, and can be ignored when reporting test results. Make this explicit in your PR test comment so the human doesn't waste time investigating.

## Devin Secrets Needed

None. The app is 100% client-side, no auth, no API keys, no backend. Tests run entirely in the local browser against a `python3 -m http.server`.

## Reusing this for the other apps

The joke app (`joke111`) and student portal (`AI-student-portal`) follow the same pattern: single `index.html`, `python3 -m http.server` to run, `localStorage` for persistence. The `localStorage.clear()` + maximize + reload setup pattern in this skill applies to all of them; just substitute the app-specific localStorage key prefix and the app-specific reference values.
