# Copilot instructions

This is a responsive, mobile-first web app that uses the SalkarsaUI design system on Next.js, Tailwind CSS v4 and shadcn/ui. Read `DESIGN.md` and `AGENTS.md` before generating UI.

- Colors: use shadcn aliases (`bg-primary`, `text-muted-foreground`) or SalkarsaUI tokens (`var(--bg-subtle)`). Don't use hex, arbitrary values or palette classes like `bg-blue-500`.
- Spacing: use the Tailwind 4px steps listed in DESIGN.md section 2.2. Typography: use the `.text-*` classes.
- Use the shadcn component mapped in DESIGN.md section 4 with SalkarsaUI variants and sizes.
- Build mobile first. Include hover, active, focus-visible and disabled states, keep touch targets at least 44px, and add `aria-label` to icon-only buttons.
- Switch dark mode with `data-theme="dark"` or `.dark`. Don't pick colors in JavaScript based on the theme.
- Write UI copy in Bahasa Indonesia, sentence case.
