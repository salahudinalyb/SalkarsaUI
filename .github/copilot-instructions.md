# Copilot instructions

This is a responsive, mobile-first web app that uses the SalkarsaUI design system on Next.js, Tailwind CSS v4 and shadcn/ui. Read `DESIGN.md` and `AGENTS.md` before generating UI. Exact sizes, radius, spacing steps and breakpoints are in `DESIGN.md`.

- Colors: use shadcn aliases (`bg-primary`, `text-muted-foreground`) or SalkarsaUI semantic tokens (`var(--bg-subtle)`). Don't use hex, arbitrary values or palette classes like `bg-blue-500`.
- Spacing: use the steps in DESIGN.md section 2.2. Typography: use the `.text-*` classes.
- Use the shadcn component mapped in DESIGN.md section 4 with the variants and sizes listed there.
- Build mobile first and include every state in DESIGN.md section 3.1. Add `aria-label` to icon-only buttons.
- Switch dark mode with `data-theme="dark"` or `.dark`. Don't pick colors in JavaScript based on the theme.
- Write UI copy as described in DESIGN.md section 5.
