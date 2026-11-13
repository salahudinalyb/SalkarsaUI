# DESIGN.md — SalkarsaUI

> Source of truth for building UI with SalkarsaUI. Written for engineers AND AI coding agents (Cursor, Claude Code, Copilot, v0…).
> Figma file: "Design" → pages "1. Foundations", "2. Components", "0. SalkarsaUI Docs".
> Platform: **mobile-first** (reference device 393×852). Font: **Inter** (UI), **Roboto Mono** (code).

---

## 0. Hard rules (read first)

1. **Never hard-code colors, spacing, radius or font values.** Always use a token (CSS var / platform constant). If no token fits, ask — don't invent one.
2. **Use semantic tokens, never primitives.** `--color-blue-500` ❌ → `--bg-brand-default` ✅. Primitives exist only to feed semantic tokens.
3. **Pair foreground with background tokens.** `bg/brand/default` → `text/on-brand`; `bg/warning/default` → `text/on-warning`; `bg/inverse` → `text/inverse`; `bg/*/subtle` → `text/<status>`.
4. **Reuse existing components** (section 4) before creating new ones. Compose with slots, don't fork.
5. **Touch target ≥ 44px** (`size/touch-target/min`). Controls are 40/48/56 tall.
6. **Every interactive element has a visible focus state** (`border/focus` + `effect/focus-ring`) and an accessible label (icon-only buttons especially).
7. **Light & dark share the same token names.** Never write `if (dark) color = …`; just use the token.
8. Spacing values come from the 4px scale only (section 2.2).

---

## 1. Token architecture

```
Primitives  (raw values: color/blue/500, space/16, radius/8, font-size/16 …)   ← DO NOT USE IN UI
   ↓ aliased by
Semantic    Color (Light) · Color Dark · Spacing · Shape · Typography          ← USE THESE
   ↓ applied in
Components  Button, Input, Card …
```

Every variable in Figma has **code syntax** set, so Dev Mode / MCP shows the exact code name:

| Figma name | Web | Android | iOS |
|---|---|---|---|
| `bg/default` | `var(--bg-default)` | `DSColor.BgDefault` | `DSColor.bgDefault` |
| `color/gray/50` (primitive) | `var(--color-gray-50)` | `DSPrimitive.ColorGray50` | `DSPrimitive.colorGray50` |

**Naming rule:** Figma `a/b/c` → CSS `--a-b-c`. Every variable also has a **description** explaining when to use it — read it in Figma or in the exported tokens.

### 1.1 Light / Dark theme
Figma (free plan) allows 1 mode per collection, so the theme is stored as **two collections with identical names and code syntax**: `Color` (light) and `Color Dark` (dark). In code, map them to ONE token set with two values:

```css
:root, [data-theme="light"] { --bg-default: #FFFFFF; /* …values from "Color" */ }
[data-theme="dark"]         { --bg-default: #0B0B0F; /* …values from "Color Dark" */ }
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) { /* same as dark */ } }
```
Ready-made: `tokens/tokens.css` already contains both themes.

---

## 2. Foundations

### 2.1 Color (semantic) — `var(--<name with dashes>)`

**Background:** `bg/default` (screen) · `bg/subtle` (secondary sections, input rest) · `bg/muted` (neutral hover, skeleton, switch track) · `bg/emphasis` · `bg/inverse` (tooltip, toast) · `bg/disabled` · `bg/overlay` (scrim) · `bg/transparent`
**Surface:** `surface/default` (card, list) · `surface/raised` (dialog, menu, sheet + shadow) · `surface/sunken` · `surface/overlay`
**Status fills:** `bg/{brand|success|warning|danger|info}/{default|hover|pressed|subtle|subtle-hover}`, `bg/neutral/{default|hover|pressed}`
**Text:** `text/primary` · `text/secondary` · `text/tertiary` · `text/placeholder` · `text/disabled` · `text/inverse` · `text/on-brand` · `text/on-warning` · `text/brand` · `text/link/{default|hover|visited}` · `text/{success|warning|danger|info}`
**Icon:** `icon/{primary|secondary|disabled|inverse|on-brand|brand|success|warning|danger|info}`
**Border:** `border/{default|subtle|strong|emphasis|disabled|inverse|brand|focus|success|warning|danger|info}`
**Effect:** `effect/shadow` · `effect/shadow-strong` · `effect/focus-ring`
**Charts:** `chart/categorical/1…8` (data viz only, use in order)

Status meaning: **brand** = primary action · **success** = positive · **warning** = caution · **danger** = destructive / error · **info** = neutral information.

### 2.2 Spacing — 4px scale (`--spacing-*`)
| token | px | typical use |
|---|---|---|
| none | 0 | |
| 4xs / 3xs | 1 / 2 | optical tweaks only |
| 2xs | 4 | tight inline gaps |
| xs | 8 | icon ↔ label, chip padding |
| sm | 12 | compact padding, label ↔ field |
| **md** | **16** | **default padding, screen side margin** |
| lg | 20 | larger containers |
| xl | 24 | between groups, dialog padding |
| 2xl / 3xl / 4xl | 32 / 40 / 48 | between sections |
| 5xl–8xl | 64 / 80 / 96 / 128 | page-level |

**Sizes:** `size/icon/{xs 12, sm 16, md 20, lg 24, xl 32, 2xl 40}` · `size/control/{xs 32, sm 40, md 48, lg 56, xl 64}` · `size/avatar/{xs 24, sm 32, md 40, lg 48, xl 64, 2xl 96}` · `size/touch-target/min 44`
**Layout (mobile):** screen 393×852 · content width 361 · grid 4 cols, gutter 16, margin 16 · safe-area top 59 / bottom 34 · status bar 54 · app bar 56 · bottom nav 64 · sheet max-height 760. In code prefer platform safe-area insets (`env(safe-area-inset-*)`).

### 2.3 Shape
**Radius** (`--radius-*`): none 0 · 2xs 2 · xs 4 (checkbox, tag) · sm 6 · **md 8 (buttons, inputs)** · lg 12 (cards) · xl 16 (dialogs) · 2xl 20 · 3xl 24 (bottom sheet top) · 4xl 32 · full 9999 (avatar, switch, pill)
**Border width:** none 0 · thin 1 (default) · thick 2 (focus/selected) · heavy 4
**Opacity:** disabled 40 · muted 60 · hover 80 · overlay 60 · full 100 · none 0

### 2.4 Typography (text styles — apply the style, not single values)
| style | font | size/line |
|---|---|---|
| display/lg · md · sm | Inter Bold | 48/56 · 44/52 · 40/48 |
| heading/xl · lg · md · sm · xs | Inter SemiBold | 36/44 · 32/40 · 28/36 · 24/32 · 20/28 |
| title/lg · md · sm | Inter SemiBold | 18/28 · 16/24 · 16/24 |
| body/lg · md · sm · xs | Inter Regular | 18/28 · 16/24 · 14/20 · 12/16 |
| label/lg · md · sm | Inter Medium | 16/24 · 14/20 · 12/16 |
| caption | Inter Regular | 12/16 |
| overline | Inter SemiBold, UPPERCASE | 11/16 |
| code | Roboto Mono Regular | 14/20 |

Use: **display** = hero only · **heading** = screen/section titles · **title** = card/list/dialog titles · **body** = content (default `body/md`) · **label** = text inside controls · **caption** = meta.
Variables: `--<style>-font-size`, `--<style>-line-height`, `--<style>-font-weight`, `--<style>-letter-spacing` (e.g. `--body-md-font-size`).

### 2.5 Elevation (effect styles)
`shadow/xs` · `shadow/sm` (card) · `shadow/md` (menu, popover) · `shadow/lg` (toast) · `shadow/xl` (dialog, bottom sheet) · `shadow/2xl` · `shadow/inner` · `focus/ring` · `blur/background-{sm|md|lg}` (frosted bars).

### 2.6 Icons
959 icons, named `Icon/<kebab-name>` (e.g. `Icon/arrow-right`, `Icon/chevron-down`), each with `Style = Regular | Filled`. Default Regular; Filled for active/selected (e.g. active Nav item). Size with `size/icon/*`, color with `icon/*`. Code: `<Icon name="arrow-right" variant="regular" size="md" />`.

---

## 3. Figma → code prop mapping

Figma property names map to camelCase props:
- Text props → string: `Label` → `label`, `Helper` → `helperText`, `Error text` → `errorText`
- `Show X` booleans → **render X when provided** (`Show helper` → pass `helperText`); don't add `showX` props unless needed.
- `Leading icon` + `Leading icon swap` → single `leadingIcon?: IconName | ReactNode`
- Swap/slot props (`Prefix`, `Suffix`, `Body`, `Footer`, `Items`, `Actions`) → `ReactNode` / children
- `Variant`, `Size`, `Color`, `Style`, `Status`, `Type` → lowercase string unions
- `State=Pressed/Focus/Open` are **interaction states** (CSS `:active`, `:focus-visible`), NOT props. `Disabled`, `Loading`, `Selected`, `Error` ARE props (`disabled`, `loading`, `selected`, `invalid`).

---

## 4. Components (33)

Format: **Name** — props → notes. Defaults in **bold**.

### Actions
- **Button** — `variant: **primary**|secondary|outline|ghost|danger`, `size: **sm**(40)|md(48)|lg(56)`, `label`, `leadingIcon`, `trailingIcon`, `prefix`, `suffix`, `loading`, `disabled`, `fullWidth`. Order: leading icon → prefix → label → suffix → trailing icon. Loading shows spinner before label and keeps the label (e.g. "Memproses…"). One primary button per view.
- **Icon button** — `icon`, `variant: **ghost**|secondary|primary`, `size: **sm**(40)|md(48)`, `disabled`, **`aria-label` required**.
- **Chip** — `label`, `leadingIcon`, `removable`, `size: **sm**|md`, `selected`, `disabled`. Filter/choice or removable tag; selected shows check.

### Inputs
- **Input** — `label`, `value`, `placeholder`, `helperText`, `errorText`, `size: **sm**|md|lg`, `leadingIcon`, `trailingIcon`, `prefix`, `suffix`, `invalid`, `disabled`. Order: leading icon → prefix (Input addon) → value → suffix → trailing icon (eye, clear, calendar) → error icon. States: default, filled, focus (border/focus + ring), error (border/danger + text/danger helper), disabled.
- **Input addon** — `text`, `type: **text**|select`, `position: **leading**|trailing`, `divider`, `size`. For +62 ▾, Rp, https://, kg.
- **Textarea** — `label`, `value`, `helperText`, `maxLength` (shows counter), `invalid`, `disabled`.
- **Select** — trigger with Input anatomy + chevron; `label`, `value`, `helperText`, `leadingIcon`, `size`, `invalid`, `disabled`. Open → Menu below (gap spacing/2xs) or Bottom sheet on mobile.
- **Checkbox** — `label`, `checked: false|true|"indeterminate"`, `disabled`. Multi-select & agreements.
- **Radio** — `label`, `checked`, `disabled`. Single choice; always in a group.
- **Switch** — `label`, `on`, `disabled`. Instant setting, no submit.
- **Segmented control** + **Segment** — `segments`, `value`; segments fill width equally. Switch views of the same content (not navigation → use Tabs).

### Feedback
- **Alert** — `status: **info**|success|warning|danger`, `title`, `description`, `actions`, `onClose`. Inline banner.
- **Toast** — `status: **neutral**|info|success|warning|danger`, `title`, `description`, `action`, `onClose`. Width 361, above bottom nav + safe area. Auto-dismiss ~4s unless it has an action.
- **Tooltip** — `text`, `placement: **top**|bottom|left|right`, `arrow`. Short labels only.
- **Badge** — `label`, `color: **neutral**|brand|success|warning|danger|info`, `style: **subtle**|solid`, `dot`. Subtle for status, solid for counts/high emphasis.
- **Spinner** — `size` 16–48 (default 24). Track 25% opacity; rotate in code. Respect `prefers-reduced-motion`.

### Navigation
- **App bar** — `type: **center**|large`, `title`, `showBack`, `trailing` (1–2 Icon buttons). Height 56, under status bar.
- **Bottom navigation** + **Nav item** — 3–5 items; Nav item: `label` (1 word), `icon`, `active`, `badge`. Active uses Filled icon + icon/brand. Includes bottom safe-area.
- **Tabs** + **Tab** — `style: **underline**|pill`, Tab: `label`, `leadingIcon`, `active`, `disabled`. Underline = page sections, Pill = compact switcher.
- **Menu** + **Menu item** — Menu item: `label`, `leadingIcon`, `selected` (shows check), `disabled`, min height 44.

### Containers
- **Card** — slots `media`, `content`, `footer`; `title`, `description`. surface/default, radius/lg.
- **Dialog** — `title`, `description`, `body`, `footer`, `onClose`. Width 328, centered on bg/overlay; footer buttons 50/50. Long content → Bottom sheet.
- **Bottom sheet** — `title`, `body`, `footer`, `showHandle`, `onClose`. Top radius 3xl, max-height 760, bottom safe-area, over bg/overlay.
- **Action sheet** + **Action sheet item** — `title`, `message`, `actions`, `cancel`; item `type: **default**|destructive|cancel`.
- **List item** — slots `leading`, `trailing`; `title`, `description`. Stack vertically for lists/settings.

### Data display
- **Avatar** — `size: **xs**|sm|md|lg|xl|2xl` (24–96), `type: initials|icon`, `initials`, `src` (photo). Fallback order: photo → initials → icon.

### System (mockup only — do NOT build in code)
- **Status bar**, **Home indicator** — use the platform's native ones.

---

## 5. Patterns

- **Screen:** Status bar → App bar → content (padding spacing/md, gap spacing/xl between groups) → Bottom navigation. Background bg/default.
- **Form:** vertical stack, gap spacing/md between fields; label above field; helper below; validate on blur, show errorText + invalid; primary Button full-width at bottom (sticky above safe-area).
- **Loading:** Button → `loading`; full content → skeleton with bg/muted; never block the whole screen with a spinner for < 1s.
- **Empty state:** illustration (page "Illustrations") + heading/sm + body/md text/secondary + one primary Button; padding spacing/4xl.
- **Destructive action:** Button variant=danger, confirm with Dialog or Action sheet (destructive item).
- **Language:** UI copy in Bahasa Indonesia, sentence case, short verbs ("Simpan", "Batal", "Memproses…").

## 6. Accessibility checklist
- Contrast: text tokens on their paired bg pass WCAG AA — don't mix arbitrary pairs.
- Touch target ≥ 44; focus visible; icon-only controls have labels.
- Don't use color alone for status — pair with icon/text (Alert, Input error do this).
- Respect dynamic type / font scaling and `prefers-reduced-motion`.

## 7. For AI agents
- Before generating UI, read sections 0, 2 and 4. Output must reference tokens (`var(--…)`), never hex/px literals except 0.
- If a needed component is missing, compose from existing ones and flag it as "candidate component" in your reply.
- When unsure which token to use, prefer the description in Figma/tokens file; pick the most semantic name (e.g. `text/secondary` over `text/tertiary` for helper text).
