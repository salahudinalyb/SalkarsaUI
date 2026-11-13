# Copilot instructions — SalkarsaUI

This repo follows the SalkarsaUI design system. Before generating UI, read `DESIGN.md` and `AGENTS.md`.

- Use semantic CSS variables from `tokens/tokens.css` for all colors, spacing, radius, borders, shadows and typography. Never hard-code hex or px values, never use primitive tokens (`--color-*`, `--space-*`).
- Reuse components and prop names from DESIGN.md §4.
- Minimum touch target 44px, visible focus state, aria-label on icon-only buttons.
- Theme with `data-theme="light|dark"`; never branch colors on dark mode in code.
- UI copy in Bahasa Indonesia, sentence case.
