# AGENTS.md

This project uses the SalkarsaUI design system for a responsive, mobile-first web app built with React or Next.js, Tailwind CSS v4 and shadcn/ui. Read `DESIGN.md` before you write or change any UI.

Token files: `tokens/tokens.css` (all tokens), `tokens/shadcn.css` (shadcn bridge) and `tokens/tokens.json` (DTCG).

## Do
- Set colors with shadcn aliases (`bg-primary`, `text-muted-foreground`, `border-border`) or with tokens (`bg-(--bg-subtle)`, `var(--text-tertiary)`).
- Take spacing from the 4px scale only. In Tailwind that means steps `1 2 3 4 5 6 8 10 12 16 20 24 32`, which match SalkarsaUI `2xs` through `8xl`.
- Set type with the `.text-*` classes (`text-body-md`, `text-heading-sm`, `text-label-md`).
- Use the shadcn component mapped in DESIGN.md section 4 and apply SalkarsaUI variants and sizes (buttons are 40, 48 or 56 tall with `rounded-lg`).
- Build mobile first: base styles for mobile, then `md:` and `lg:`. Show the bottom nav only below `md` and use a sidebar or top nav on desktop. Dialogs become drawers on mobile.
- Implement every state in DESIGN.md section 3.1: hover, focus-visible, active, disabled, loading and error. Use `--focus-ring-offset` on controls and `--focus-ring` on text fields. Touch targets are at least 44px on mobile.
- Follow the responsive swaps in DESIGN.md section 2.3: bottom nav to sidebar at `md`, app bar to navbar at `lg`, drawer to dialog at `md`, and lists or cards to a table at `md`.
- Switch dark mode with `data-theme="dark"` or `.dark` on `<html>`. Both themes use the same token names.
- Write UI copy in Bahasa Indonesia, sentence case ("Simpan", "Batal", "Memproses…").

## Don't
- Use hex or rgb values, arbitrary values (`p-[13px]`, `text-[#333]`) or palette classes (`bg-blue-500`, `text-gray-600`).
- Use primitive tokens (`--color-*`, `--space-*`) in UI code.
- Pick colors in JavaScript based on dark mode.
- Edit files in `tokens/` by hand. They are generated from Figma.
- Build the status bar or home indicator. Those exist in Figma for mockups only.

## If you're unsure
- Choose the most semantic token and check its `$description` in `tokens/tokens.json`.
- If a component is missing, compose it from the ones in DESIGN.md section 4 or restyle the shadcn default with tokens, and mention "candidate component: <name>" in your reply.

## Before you finish
- [ ] `grep -rnE "#[0-9a-fA-F]{3,8}\b|\[[0-9]+px\]|(bg|text|border)-(gray|slate|zinc|blue|red|green|yellow)-[0-9]" src app components` finds nothing outside the token files
- [ ] Works at 375px, 768px and 1280px
- [ ] Works in light and dark
- [ ] Focus is visible and icon-only buttons have labels
