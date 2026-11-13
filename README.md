# SalkarsaUI

SalkarsaUI is a mobile-first design system. It includes a Figma library, design tokens for light and dark themes, and written guidelines you can hand to your team or to an AI coding assistant.

What's included:

- 33 components and 959 icons (Regular and Filled)
- 552 tokens, from raw primitives up to semantic names, in light and dark
- A short note on every token and component explaining when to use it
- Code names for Web (CSS variables), Android and iOS

## Repo contents

| Path | Contents |
|---|---|
| `DESIGN.md` | The full guide: rules, tokens, components and patterns. Start here. |
| `tokens/tokens.css` | CSS custom properties with light/dark themes, shadows and `.text-*` classes |
| `tokens/tokens.json` | The same tokens in W3C DTCG format, for Style Dictionary, Tailwind or native builds |
| `AGENTS.md` | Short rules for coding agents (Codex, Cursor, Copilot, Gemini and others) |
| `CLAUDE.md` | Entry file for Claude Code |
| `.cursor/rules/salkarsaui.mdc` | Rule file for Cursor |
| `.github/copilot-instructions.md` | Instructions for GitHub Copilot |
| `llms.txt` | Index of this repo for LLM tools |

## Getting started (web)

1. Add `tokens/tokens.css` to your project:
   ```bash
   curl -o src/styles/tokens.css https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/tokens/tokens.css
   ```
2. Import it once at the root of your app:
   ```css
   @import "./tokens.css";
   body { background: var(--bg-default); color: var(--text-primary); }
   ```
3. Set the theme with `<html data-theme="dark">`. Leave the attribute off to follow the OS setting.
4. Use tokens instead of raw values:
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
// tailwind.config.js: point Tailwind utilities at the tokens
theme: { extend: {
  colors: { bg: { DEFAULT: 'var(--bg-default)', subtle: 'var(--bg-subtle)' }, text: { primary: 'var(--text-primary)' } },
  spacing: { xs: 'var(--spacing-xs)', sm: 'var(--spacing-sm)', md: 'var(--spacing-md)', lg: 'var(--spacing-lg)', xl: 'var(--spacing-xl)' },
  borderRadius: { md: 'var(--radius-md)', lg: 'var(--radius-lg)' },
}}
```

## Using it with AI coding tools

Copy `DESIGN.md` and `AGENTS.md` into the root of your app's repo, then set up your tool:

| Tool | Setup |
|---|---|
| Claude Code | Add `@DESIGN.md` to your `CLAUDE.md`, or copy ours |
| Codex, Gemini CLI, Jules | They read `AGENTS.md` on their own |
| Cursor | Copy `.cursor/rules/salkarsaui.mdc` |
| GitHub Copilot | Copy `.github/copilot-instructions.md` |
| v0, Bolt, Lovable | Paste `DESIGN.md` into the project knowledge or custom instructions |

You can also point your agent straight at the file: *"Read https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/DESIGN.md and follow it for all UI work."*

## Figma

Figma library link: coming soon.

Pages: `0. SalkarsaUI Docs` (usage docs), `1. Foundations`, `2. Components`, `Icons`, `Illustrations`.

In Dev Mode, each variable shows its code name, for example `var(--bg-default)`, `DSColor.BgDefault` or `DSColor.bgDefault`.

## Updating tokens

The files in `tokens/` are generated from the Figma variables. To change a token, edit it in Figma, export again, then update `tokens/` and `CHANGELOG.md`. Don't change token values in code only.

## License

[MIT](LICENSE)
