# AGENTS.md

This project uses the SalkarsaUI design system for a responsive, mobile-first web app built with React or Next.js, Tailwind CSS v4 and shadcn/ui. Read `DESIGN.md` before you write or change any UI. All exact values (sizes, radius, spacing steps, breakpoints) are there, so look them up instead of guessing.

Token files: `tokens/tokens.css` (all tokens), `tokens/shadcn.css` (shadcn bridge) and `tokens/tokens.json` (DTCG).

## Do
- Set colors with shadcn aliases (`bg-primary`, `text-muted-foreground`, `border-border`) or with semantic tokens (`bg-(--bg-subtle)`, `var(--text-tertiary)`).
- Take spacing from the steps in DESIGN.md section 2.2 and set type with the `.text-*` classes.
- Use the shadcn component mapped in DESIGN.md section 4, with the variants and sizes listed there.
- Build mobile first: base styles for mobile, then `md:` and `lg:`, and swap components as shown in DESIGN.md section 2.3.
- Implement every state in DESIGN.md section 3.1 and the accessibility rules in section 6.
- Switch dark mode with `data-theme="dark"` or `.dark` on `<html>`. Both themes use the same token names.
- Write UI copy as described in DESIGN.md section 5.

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
