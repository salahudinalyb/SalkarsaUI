# Copilot instructions

This repo uses the SalkarsaUI design system. Read `DESIGN.md` and `AGENTS.md` before generating UI.

- Use the semantic CSS variables in `tokens/tokens.css` for colors, spacing, radius, borders, shadows and typography. Don't hard-code hex or px values, and don't use primitive tokens (`--color-*`, `--space-*`).
- Reuse the components and prop names listed in DESIGN.md section 4.
- Keep touch targets at least 44px, show a visible focus state, and add `aria-label` to icon-only buttons.
- Set the theme with `data-theme="light|dark"`. Don't switch colors on dark mode in code.
- Write UI copy in Bahasa Indonesia, sentence case.
