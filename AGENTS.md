# AGENTS.md — SalkarsaUI

This project uses the **SalkarsaUI** design system. Full spec: `DESIGN.md` (read it before writing or changing any UI). Tokens: `tokens/tokens.css`, `tokens/tokens.json`.

## Must
- Use **semantic tokens** for every color, spacing, radius, border, shadow and font value: `var(--bg-default)`, `var(--spacing-md)`, `var(--radius-md)`, `.text-body-md`.
- Pair foreground/background tokens: `bg-brand-default` → `text-on-brand`; `bg-warning-default` → `text-on-warning`; `bg-inverse` → `text-inverse`; `bg-<status>-subtle` → `text-<status>`.
- Reuse the components listed in DESIGN.md §4 and their prop names/variants. Compose via slots instead of creating look-alikes.
- Interactive targets ≥ 44px; visible focus (`--border-focus`, `--effect-focus-ring`); `aria-label` on icon-only buttons.
- Theme via `data-theme="light|dark"` on `<html>`. Light and dark share the same token names.
- UI copy in Bahasa Indonesia, sentence case ("Simpan", "Batal", "Memproses…").

## Never
- Hex/rgb/px literals in UI code (except `0`), or primitive tokens like `--color-blue-500` / `--space-16`.
- `if (dark)` color branching.
- Editing `tokens/*` by hand — tokens are generated from Figma.
- Building Status bar / Home indicator in code (Figma mockup only).

## When unsure
- Pick the most semantic token; read its `$description` in `tokens/tokens.json`.
- If a component is missing, compose from existing ones and say "candidate component: <name>" in your response.

## Checks before finishing
- [ ] No raw colors/sizes (`grep -nE "#[0-9a-fA-F]{3,8}|[0-9]+px" src` returns only token files)
- [ ] Works in light and dark
- [ ] Touch targets, focus states, labels present
