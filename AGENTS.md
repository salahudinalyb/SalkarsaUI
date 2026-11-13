# AGENTS.md

This project uses the SalkarsaUI design system. Read `DESIGN.md` before you write or change any UI. Tokens live in `tokens/tokens.css` and `tokens/tokens.json`.

## Do
- Use semantic tokens for every color, spacing, radius, border, shadow and font value, such as `var(--bg-default)`, `var(--spacing-md)`, `var(--radius-md)` and `.text-body-md`.
- Pair foreground and background tokens: `bg-brand-default` with `text-on-brand`, `bg-warning-default` with `text-on-warning`, `bg-inverse` with `text-inverse`, and `bg-<status>-subtle` with `text-<status>`.
- Reuse the components in DESIGN.md section 4 and keep their prop names and variants. Build new layouts by composing them through slots, not by making look-alike components.
- Keep interactive targets at least 44px. Show a visible focus state (`--border-focus` and `--effect-focus-ring`) and give icon-only buttons an `aria-label`.
- Set the theme with `data-theme="light|dark"` on `<html>`. Both themes use the same token names.
- Write UI copy in Bahasa Indonesia, sentence case ("Simpan", "Batal", "Memproses…").

## Don't
- Write hex, rgb or px values in UI code (`0` is fine), or use primitive tokens like `--color-blue-500` or `--space-16`.
- Branch colors on dark mode in code (`if (dark) …`).
- Edit files in `tokens/` by hand. They are generated from Figma.
- Build the status bar or home indicator in code. Those exist in Figma for mockups only.

## If you're unsure
- Choose the most semantic token and check its `$description` in `tokens/tokens.json`.
- If a component doesn't exist yet, compose it from existing ones and mention "candidate component: <name>" in your reply.

## Before you finish
- [ ] No raw colors or sizes (`grep -nE "#[0-9a-fA-F]{3,8}|[0-9]+px" src` should only match token files)
- [ ] Works in both light and dark
- [ ] Touch targets, focus states and labels are in place
