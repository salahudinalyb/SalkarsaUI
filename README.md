# SalkarsaUI

SalkarsaUI is a mobile-first design system for the web. It comes with a Figma library, design tokens, a ready-made theme for shadcn/ui, and written guidelines you can give to your team or to an AI coding assistant.

The goal is simple: whether a person or an agent (Claude Code, Codex, Cursor, Copilot, v0, Lovable, Bolt) builds the screen, it should come out on brand and responsive without guesswork.

What you get:

- Responsive layouts from mobile web to tablet and desktop (breakpoints at 640, 768, 1024, 1280 and 1536)
- A drop-in theme for shadcn/ui and Tailwind CSS v4
- 51 components, each mapped to a shadcn/ui component, and 959 icons (Regular and Filled)
- 573 tokens, from raw primitives up to semantic names, in light and dark
- Hover, focus, pressed and disabled states on every component, plus a doc page for each one in Figma
- A short note on every token and component explaining when to use it

## Repo contents

| Path | Contents |
|---|---|
| `DESIGN.md` | The full guide: rules, tokens, breakpoints, states, the component to shadcn mapping, and patterns. Start here. |
| `SKILL.md` | Makes this repo an Agent Skill, so you can install it once and use it in every project |
| `tokens/tokens.css` | All tokens as CSS variables, with light/dark themes, shadows, focus rings and `.text-*` classes |
| `tokens/shadcn.css` | Points the shadcn/ui variables at SalkarsaUI tokens (Tailwind v4) |
| `tokens/tokens.json` | The same tokens in W3C DTCG format, with descriptions |
| `AGENTS.md` | Rules for coding agents (Codex, Cursor, Copilot, Gemini and others) |
| `CLAUDE.md` | Entry file for Claude Code |
| `.cursor/rules/salkarsaui.mdc` | Rule file for Cursor |
| `.github/copilot-instructions.md` | Instructions for GitHub Copilot |
| `llms.txt` | Index of this repo for LLM tools |
| `CHANGELOG.md` | Version history |

## Getting started with Next.js, Tailwind v4 and shadcn/ui

```bash
npx shadcn@latest init
curl -o app/tokens.css https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/tokens/tokens.css
curl -o app/shadcn.css https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/tokens/shadcn.css
```

```css
/* app/globals.css: remove the color variables shadcn generated, then add */
@import "tailwindcss";
@import "./tokens.css";
@import "./shadcn.css";
```

Then update `cn()` in `lib/utils.ts` as shown in DESIGN.md section 1, so it keeps the `.text-*` type classes. Every shadcn component now picks up SalkarsaUI colors, radius and fonts:

```tsx
<Button>Simpan</Button>
<Button variant="destructive">Hapus</Button>
<h1 className="text-heading-sm lg:text-heading-lg">Ringkasan</h1>
<p className="text-body-md text-muted-foreground">Halo!</p>
```

For dark mode, set `<html data-theme="dark">` or `class="dark"` (this works with next-themes). Without either, the theme follows the OS.

### Plain CSS or other frameworks
```css
@import "./tokens.css";
.card {
  background: var(--surface-default);
  border: var(--border-width-thin) solid var(--border-default);
  border-radius: var(--radius-lg);
  padding: var(--spacing-md);
  box-shadow: var(--shadow-sm);
}
.button:focus-visible { box-shadow: var(--focus-ring-offset); }
```

## Using it with AI coding tools

### Install as a skill (recommended)

The whole repo is an [Agent Skill](https://agentskills.io). Install it once and your agent picks it up on its own whenever it works on UI, in any project. If a project doesn't have the tokens yet, the agent copies them in for you.

```bash
# Claude Code
git clone https://github.com/salahudinalyb/SalkarsaUI ~/.claude/skills/salkarsaui

# Codex and other tools that read ~/.agents/skills
git clone https://github.com/salahudinalyb/SalkarsaUI ~/.agents/skills/salkarsaui
```

Not comfortable with the terminal? Paste this into your agent and it will do the install for you:

> Install the SalkarsaUI skill from https://github.com/salahudinalyb/SalkarsaUI

To get the latest version later:

```bash
git -C ~/.claude/skills/salkarsaui pull
```

### Add it to a single project

For tools that don't support skills yet, copy `DESIGN.md` and `AGENTS.md` into the root of your app's repo, then set up your tool:

| Tool | Setup |
|---|---|
| Claude Code | Add `@DESIGN.md` to your `CLAUDE.md`, or copy ours |
| Codex, Gemini CLI, Jules | They read `AGENTS.md` on their own |
| Cursor | Copy `.cursor/rules/salkarsaui.mdc` |
| GitHub Copilot | Copy `.github/copilot-instructions.md` |
| v0, Lovable, Bolt | Paste `DESIGN.md` into the project knowledge or custom instructions, and add `tokens.css` and `shadcn.css` |

You can also point your agent straight at the file: *"Read https://raw.githubusercontent.com/salahudinalyb/SalkarsaUI/main/DESIGN.md and follow it for all UI work."*

## Figma

Figma library link: coming soon.

Pages: `0. SalkarsaUI Docs` (foundations plus a doc page for each component), `1. Foundations`, `2. Components`, `Icons`, `Illustrations`.

In Dev Mode, each variable shows its CSS name, for example `var(--bg-default)`. Grid styles: `grid/mobile`, `grid/tablet`, `grid/desktop` and `grid/desktop-1440`.

## Roadmap
- [x] Tokens, light/dark themes, shadcn bridge and AI guidelines
- [x] Breakpoints, grids, desktop layout tokens and responsive rules
- [x] Hover and focus states on interactive components
- [x] Desktop components: Navbar, Sidebar, Dialog sizes, Table, Pagination, Breadcrumb, Popover, Date picker, Skeleton, Progress
- [ ] React component package `@salkarsa/ui`
- [ ] Icon package

## Updating tokens
The files in `tokens/` are generated from the Figma variables. To change a token, edit it in Figma first, regenerate `tokens/`, then update `CHANGELOG.md`. Don't change token values in code only.

Exact values and component specs are written only in `DESIGN.md`. `SKILL.md`, `AGENTS.md` and the Cursor and Copilot rules point to it, so a change in Figma means updating `DESIGN.md` (and `tokens/` if a token changed), and nothing else.

## License

[MIT](LICENSE)
