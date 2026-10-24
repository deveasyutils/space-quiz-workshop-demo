# Copilot Instructions

## Project
A single-file Korean-language space exploration quiz: everything (HTML, CSS, JS) lives in `index.html`. No server, no build step, no package manager, no tests or linters. Run it by opening `index.html` in a browser.

## Architecture (inside `index.html`)
- `QUESTIONS` array: `{ q, a: [4 options], c: correctIndex, e: explanation }`. Counts are derived from `QUESTIONS.length` at render time, but the markup's `aria-valuemax="10"` on `#pbar` must be updated by hand if the question count changes.
- `RESULTS` array: `[minScore, emoji, title, message]`, ordered high to low; `finish()` picks the first entry where `score >= minScore`.
- Flow: `start()` → `render()` (builds option buttons) → `choose(n)` (feedback, score, progress) → `next()` → `finish()`. Two `<section>`s (`#quiz`, `#result`) are toggled with the `hidden` attribute.

## Conventions
- **No external dependencies**: no CDN scripts, fonts, or network requests.
- **Theming**: colors are CSS custom properties in `:root`, overridden in a `@media (prefers-color-scheme: dark)` block. Use the variables; don't hardcode colors in new rules.
- **Accessibility must be preserved**: keyboard-only play (keys 1–4 select, Enter activates the focused "next" button), focus is moved programmatically (question heading on render, next button after answering, result title on finish), feedback uses `role="status"`/`aria-live="polite"`, decorative emoji get `aria-hidden`, and animations are disabled under `prefers-reduced-motion`.
- UI text is Korean; keep `lang="ko"`.
- Question/option text is set via `textContent`, not `innerHTML`.
