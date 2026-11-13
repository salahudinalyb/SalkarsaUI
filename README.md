# SalkarsaUI

Mobile-first design system — Figma library + design tokens + AI-ready guidelines.
Built so engineers **and** AI coding agents (Claude Code, Codex, Cursor, Copilot, v0…) produce on-brand UI without guessing.

- 33 components · 959 icons (Regular / Filled)
- 552 tokens (primitive → semantic), light & dark theme
- Every token and component documented with *when to use it*
- Code names for Web (CSS vars), Android and iOS

## What's in this repo

| Path | What it is |
|---|---|
| `DESIGN.md` | **The source of truth.** Rules, tokens, components, patterns. Read this first. |
| `tokens/tokens.css` | CSS custom properties incl. light/dark theme, shadows, `.text-*` classes |
| `tokens/tokens.json` | Same tokens in W3C DTCG format (Style Dictionary, Tailwind, native builds) |
| `AGENTS.md` | Instructions for AI coding agents (Codex, Cursor, Copilot, Gemini, …) |
| `CLAUDE.md` | Entry point for Claude Code |
| `.cursor/rules/salkarsaui.mdc` | Cursor rule |
| `.github/copilot-instructions.md` | GitHub Copilot instructions |
| `llms.txt` | Index for LLMs / AI tools fetching this repo |

## Quick start (web)

1. Copy `tokens/tokens.css` into your project (or download it):
   ```bash
   curl -o src/styles/tokens.css https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/tokens/tokens.css
   ```
2. Import once at the app root:
   ```css
   @import "./tokens.css";
   body { background: var(--bg-default); color: var(--text-primary); }
   ```
3. Switch theme: `<html data-theme="dark">` (no attribute = follow OS).
4. Use tokens, never raw values:
   ```css
   .card {
     background: var(--surface-default);
     border: var(--border-width-thin) solid var(--border-default);
     border-radius: var(--radius-lg);
     padding: var(--spacing-md);
     box-shadow: var(--shadow-sm);
   }
   ```
   ```html
   <h2 class="text-heading-sm">Ringkasan</h2>
   <p class="text-body-md">Halo!</p>
   ```

### Tailwind (optional)
```js
// tailwind.config.js — map utilities to tokens
theme: { extend: {
  colors: { bg: { DEFAULT: 'var(--bg-default)', subtle: 'var(--bg-subtle)' }, text: { primary: 'var(--text-primary)' } },
  spacing: { xs: 'var(--spacing-xs)', sm: 'var(--spacing-sm)', md: 'var(--spacing-md)', lg: 'var(--spacing-lg)', xl: 'var(--spacing-xl)' },
  borderRadius: { md: 'var(--radius-md)', lg: 'var(--radius-lg)' },
}}
```

## Using SalkarsaUI with AI coding tools

Copy `DESIGN.md` and `AGENTS.md` into the root of **your app's repo**, then:

| Tool | Do this |
|---|---|
| Claude Code | Add `@DESIGN.md` to your `CLAUDE.md` (or copy ours) |
| Codex / Gemini CLI / Jules | Reads `AGENTS.md` automatically |
| Cursor | Copy `.cursor/rules/salkarsaui.mdc` |
| GitHub Copilot | Copy `.github/copilot-instructions.md` |
| v0 / Bolt / Lovable | Paste `DESIGN.md` into project knowledge / custom instructions |

Or just tell your agent: *"Fetch https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/DESIGN.md and follow it for all UI."*

## Figma

Figma library: _add your Community / library link here_.
Pages: `0. SalkarsaUI Docs` (usage docs) · `1. Foundations` · `2. Components` · `Icons` · `Illustrations`.
In Dev Mode, every variable shows its code name (`var(--bg-default)`, `DSColor.BgDefault`, `DSColor.bgDefault`).

## Updating tokens

Tokens are generated from Figma variables. Change them in Figma first, re-export, then update `tokens/` and `CHANGELOG.md`. Never edit token values only in code.

## License

_TBD_
