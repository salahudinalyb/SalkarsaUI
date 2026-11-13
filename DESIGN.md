# DESIGN.md

This is the main reference for building UI with SalkarsaUI. It's written for developers and for AI coding tools such as Cursor, Claude Code, Copilot and v0.

- Figma file: "Design", pages "1. Foundations", "2. Components" and "0. SalkarsaUI Docs"
- Platform: mobile first, with a reference device of 393×852
- Fonts: Inter for UI, Roboto Mono for code

---

## 0. Core rules

1. Don't hard-code colors, spacing, radius or font values. Use a token (CSS variable or platform constant). If no token fits, ask instead of making one up.
2. Use semantic tokens, not primitives. Write `--bg-brand-default`, not `--color-blue-500`. Primitives only exist to feed the semantic tokens.
3. Pair foreground and background tokens: `bg/brand/default` with `text/on-brand`, `bg/warning/default` with `text/on-warning`, `bg/inverse` with `text/inverse`, and `bg/*/subtle` with `text/<status>`.
4. Check the components in section 4 before building a new one. Compose with slots instead of forking.
5. Touch targets are at least 44px (`size/touch-target/min`). Controls come in 40, 48 and 56 heights.
6. Every interactive element needs a visible focus state (`border/focus` plus `effect/focus-ring`) and an accessible label. This matters most for icon-only buttons.
7. Light and dark use the same token names. Don't write `if (dark) color = …`, just use the token.
8. Spacing values come from the 4px scale only (section 2.2).

---

## 1. How tokens are organized

```
Primitives  (raw values: color/blue/500, space/16, radius/8, font-size/16 …)   ← not for UI
   ↓ referenced by
Semantic    Color (Light) · Color Dark · Spacing · Shape · Typography          ← use these
   ↓ used in
Components  Button, Input, Card …
```

Each variable in Figma has code syntax set, so Dev Mode and the Figma MCP show its exact code name:

| Figma name | Web | Android | iOS |
|---|---|---|---|
| `bg/default` | `var(--bg-default)` | `DSColor.BgDefault` | `DSColor.bgDefault` |
| `color/gray/50` (primitive) | `var(--color-gray-50)` | `DSPrimitive.ColorGray50` | `DSPrimitive.colorGray50` |

Naming: Figma `a/b/c` becomes CSS `--a-b-c`. Each variable also has a description saying when to use it. You can read it in Figma or in the exported tokens.

### 1.1 Light and dark themes
The Figma free plan only allows one mode per collection, so the themes are stored as two collections with the same names and code syntax: `Color` (light) and `Color Dark` (dark). In code they become one token set with two values:

```css
:root, [data-theme="light"] { --bg-default: #FFFFFF; /* …values from "Color" */ }
[data-theme="dark"]         { --bg-default: #0B0B0F; /* …values from "Color Dark" */ }
@media (prefers-color-scheme: dark) { :root:not([data-theme="light"]) { /* same as dark */ } }
```
`tokens/tokens.css` already includes both themes.

---

## 2. Foundations

### 2.1 Color (semantic), used as `var(--<name with dashes>)`

**Background:** `bg/default` (screen), `bg/subtle` (secondary sections, resting inputs), `bg/muted` (neutral hover, skeleton, switch track), `bg/emphasis`, `bg/inverse` (tooltip, toast), `bg/disabled`, `bg/overlay` (scrim), `bg/transparent`

**Surface:** `surface/default` (card, list), `surface/raised` (dialog, menu, sheet, with shadow), `surface/sunken`, `surface/overlay`

**Status fills:** `bg/{brand|success|warning|danger|info}/{default|hover|pressed|subtle|subtle-hover}`, `bg/neutral/{default|hover|pressed}`

**Text:** `text/primary`, `text/secondary`, `text/tertiary`, `text/placeholder`, `text/disabled`, `text/inverse`, `text/on-brand`, `text/on-warning`, `text/brand`, `text/link/{default|hover|visited}`, `text/{success|warning|danger|info}`

**Icon:** `icon/{primary|secondary|disabled|inverse|on-brand|brand|success|warning|danger|info}`

**Border:** `border/{default|subtle|strong|emphasis|disabled|inverse|brand|focus|success|warning|danger|info}`

**Effect:** `effect/shadow`, `effect/shadow-strong`, `effect/focus-ring`

**Charts:** `chart/categorical/1` to `8`, for data visualization only, used in order

What the status colors mean: brand is the primary action, success is positive, warning means caution, danger is for destructive actions and errors, and info is neutral information.

### 2.2 Spacing (4px scale, `--spacing-*`)
| Token | px | Typical use |
|---|---|---|
| none | 0 | |
| 4xs / 3xs | 1 / 2 | small optical adjustments |
| 2xs | 4 | tight inline gaps |
| xs | 8 | icon to label, chip padding |
| sm | 12 | compact padding, label to field |
| **md** | **16** | **default padding and screen side margin** |
| lg | 20 | larger containers |
| xl | 24 | between groups, dialog padding |
| 2xl / 3xl / 4xl | 32 / 40 / 48 | between sections |
| 5xl to 8xl | 64 / 80 / 96 / 128 | page level |

**Sizes:** `size/icon/{xs 12, sm 16, md 20, lg 24, xl 32, 2xl 40}`, `size/control/{xs 32, sm 40, md 48, lg 56, xl 64}`, `size/avatar/{xs 24, sm 32, md 40, lg 48, xl 64, 2xl 96}`, `size/touch-target/min 44`

**Layout (mobile):** screen 393×852, content width 361, 4-column grid with 16 gutter and 16 margin, safe area 59 top and 34 bottom, status bar 54, app bar 56, bottom nav 64, sheet max height 760. In code, use the platform's safe-area insets (`env(safe-area-inset-*)`) instead of fixed numbers.

### 2.3 Shape
**Radius** (`--radius-*`): none 0, 2xs 2, xs 4 (checkbox, tag), sm 6, **md 8 (buttons, inputs)**, lg 12 (cards), xl 16 (dialogs), 2xl 20, 3xl 24 (top of bottom sheet), 4xl 32, full 9999 (avatar, switch, pill)

**Border width:** none 0, thin 1 (default), thick 2 (focus, selected), heavy 4

**Opacity:** disabled 40, muted 60, hover 80, overlay 60, full 100, none 0

### 2.4 Typography
Apply the text style as a whole rather than setting single values.

| Style | Font | Size / line height |
|---|---|---|
| display/lg, md, sm | Inter Bold | 48/56, 44/52, 40/48 |
| heading/xl, lg, md, sm, xs | Inter SemiBold | 36/44, 32/40, 28/36, 24/32, 20/28 |
| title/lg, md, sm | Inter SemiBold | 18/28, 16/24, 16/24 |
| body/lg, md, sm, xs | Inter Regular | 18/28, 16/24, 14/20, 12/16 |
| label/lg, md, sm | Inter Medium | 16/24, 14/20, 12/16 |
| caption | Inter Regular | 12/16 |
| overline | Inter SemiBold, uppercase | 11/16 |
| code | Roboto Mono Regular | 14/20 |

When to use each: display for hero areas only, heading for screen and section titles, title for card, list and dialog titles, body for content (`body/md` by default), label for text inside controls, and caption for metadata.

Variables follow the pattern `--<style>-font-size`, `--<style>-line-height`, `--<style>-font-weight` and `--<style>-letter-spacing`, for example `--body-md-font-size`.

### 2.5 Elevation (effect styles)
`shadow/xs`, `shadow/sm` (card), `shadow/md` (menu, popover), `shadow/lg` (toast), `shadow/xl` (dialog, bottom sheet), `shadow/2xl`, `shadow/inner`, `focus/ring`, `blur/background-{sm|md|lg}` (frosted bars).

### 2.6 Icons
There are 959 icons named `Icon/<kebab-name>` (for example `Icon/arrow-right` or `Icon/chevron-down`), each with `Style = Regular | Filled`. Regular is the default. Use Filled for active or selected states, like the active Nav item. Size icons with `size/icon/*` and color them with `icon/*`. In code: `<Icon name="arrow-right" variant="regular" size="md" />`.

---

## 3. Mapping Figma properties to code props

Figma property names become camelCase props:
- Text properties become strings: `Label` → `label`, `Helper` → `helperText`, `Error text` → `errorText`.
- `Show X` booleans mean "render X when it's provided". For `Show helper`, just pass `helperText`. Only add a `showX` prop if you really need one.
- `Leading icon` and `Leading icon swap` combine into one `leadingIcon?: IconName | ReactNode`.
- Swap and slot properties (`Prefix`, `Suffix`, `Body`, `Footer`, `Items`, `Actions`) become `ReactNode` or children.
- `Variant`, `Size`, `Color`, `Style`, `Status` and `Type` become lowercase string unions.
- `State=Pressed/Focus/Open` are interaction states handled in CSS (`:active`, `:focus-visible`), not props. `Disabled`, `Loading`, `Selected` and `Error` are props (`disabled`, `loading`, `selected`, `invalid`).

---

## 4. Components (33)

Each entry lists the props, then notes. Defaults are in bold.

### Actions
- **Button**: `variant: **primary**|secondary|outline|ghost|danger`, `size: **sm**(40)|md(48)|lg(56)`, `label`, `leadingIcon`, `trailingIcon`, `prefix`, `suffix`, `loading`, `disabled`, `fullWidth`. Content order is leading icon, prefix, label, suffix, trailing icon. While loading, a spinner appears before the label and the label stays visible (for example "Memproses…"). Use one primary button per view.
- **Icon button**: `icon`, `variant: **ghost**|secondary|primary`, `size: **sm**(40)|md(48)`, `disabled`. An `aria-label` is required.
- **Chip**: `label`, `leadingIcon`, `removable`, `size: **sm**|md`, `selected`, `disabled`. Works as a filter, a choice or a removable tag. Selected chips show a check.

### Inputs
- **Input**: `label`, `value`, `placeholder`, `helperText`, `errorText`, `size: **sm**|md|lg`, `leadingIcon`, `trailingIcon`, `prefix`, `suffix`, `invalid`, `disabled`. Content order is leading icon, prefix (Input addon), value, suffix, trailing icon (eye, clear, calendar), then the error icon. States: default, filled, focus (border/focus plus ring), error (border/danger with a text/danger helper) and disabled.
- **Input addon**: `text`, `type: **text**|select`, `position: **leading**|trailing`, `divider`, `size`. For things like +62 ▾, Rp, https:// or kg.
- **Textarea**: `label`, `value`, `helperText`, `maxLength` (shows a counter), `invalid`, `disabled`.
- **Select**: a trigger with the same anatomy as Input plus a chevron. Props: `label`, `value`, `helperText`, `leadingIcon`, `size`, `invalid`, `disabled`. When open, it shows a Menu below the trigger (gap spacing/2xs), or a Bottom sheet on mobile.
- **Checkbox**: `label`, `checked: false|true|"indeterminate"`, `disabled`. For multi-select and agreements.
- **Radio**: `label`, `checked`, `disabled`. For single choice, always inside a group.
- **Switch**: `label`, `on`, `disabled`. For settings that apply right away, without a submit button.
- **Segmented control** and **Segment**: `segments`, `value`. Segments share the width equally. Use it to switch between views of the same content. For navigation, use Tabs.

### Feedback
- **Alert**: `status: **info**|success|warning|danger`, `title`, `description`, `actions`, `onClose`. An inline banner.
- **Toast**: `status: **neutral**|info|success|warning|danger`, `title`, `description`, `action`, `onClose`. 361 wide, shown above the bottom nav and safe area. Closes on its own after about 4 seconds unless it has an action.
- **Tooltip**: `text`, `placement: **top**|bottom|left|right`, `arrow`. Keep the text short.
- **Badge**: `label`, `color: **neutral**|brand|success|warning|danger|info`, `style: **subtle**|solid`, `dot`. Use subtle for status and solid for counts or anything that needs more emphasis.
- **Spinner**: `size` from 16 to 48 (default 24). The track is at 25% opacity, and the rotation is done in code. Respect `prefers-reduced-motion`.

### Navigation
- **App bar**: `type: **center**|large`, `title`, `showBack`, `trailing` (one or two Icon buttons). 56 tall, placed under the status bar.
- **Bottom navigation** and **Nav item**: 3 to 5 items. Nav item props are `label` (one word), `icon`, `active` and `badge`. The active item uses the Filled icon with icon/brand. Includes the bottom safe area.
- **Tabs** and **Tab**: `style: **underline**|pill`. Tab props are `label`, `leadingIcon`, `active` and `disabled`. Underline tabs are for page sections, pill tabs are a compact switcher.
- **Menu** and **Menu item**: Menu item props are `label`, `leadingIcon`, `selected` (shows a check) and `disabled`. Minimum height is 44.

### Containers
- **Card**: slots `media`, `content` and `footer`, plus `title` and `description`. Uses surface/default and radius/lg.
- **Dialog**: `title`, `description`, `body`, `footer`, `onClose`. 328 wide, centered on bg/overlay, with footer buttons split 50/50. For long content, use a Bottom sheet.
- **Bottom sheet**: `title`, `body`, `footer`, `showHandle`, `onClose`. Top corners use radius 3xl, max height is 760, includes the bottom safe area, and sits on bg/overlay.
- **Action sheet** and **Action sheet item**: `title`, `message`, `actions`, `cancel`. Item `type: **default**|destructive|cancel`.
- **List item**: slots `leading` and `trailing`, plus `title` and `description`. Stack them vertically for lists and settings.

### Data display
- **Avatar**: `size: **xs**|sm|md|lg|xl|2xl` (24 to 96), `type: initials|icon`, `initials`, `src` (photo). Falls back from photo to initials to icon.

### System (Figma mockups only, don't build these in code)
- **Status bar** and **Home indicator**: use the platform's native ones.

---

## 5. Patterns

- **Screen:** status bar, app bar, content (padding spacing/md, gap spacing/xl between groups), then bottom navigation. Background is bg/default.
- **Form:** a vertical stack with spacing/md between fields. Labels go above the field and helper text below it. Validate on blur and show `errorText` with `invalid`. The primary Button is full width at the bottom, sticky above the safe area.
- **Loading:** use `loading` on the Button. For whole sections of content, show a skeleton with bg/muted. Don't block the whole screen with a spinner for anything under a second.
- **Empty state:** an illustration (from the "Illustrations" page), a heading/sm title, body/md text in text/secondary, and one primary Button. Padding is spacing/4xl.
- **Destructive action:** use Button with variant=danger, and confirm with a Dialog or an Action sheet (destructive item).
- **Language:** UI copy in Bahasa Indonesia, sentence case, with short verbs ("Simpan", "Batal", "Memproses…").

## 6. Accessibility checklist
- Contrast: text tokens on their paired backgrounds meet WCAG AA. Don't mix arbitrary pairs.
- Touch targets are at least 44, focus is visible, and icon-only controls have labels.
- Don't rely on color alone to show status. Add an icon or text, as Alert and Input errors do.
- Support dynamic type and font scaling, and respect `prefers-reduced-motion`.

## 7. Notes for AI tools
- Read sections 0, 2 and 4 before generating UI. Use tokens (`var(--…)`) in the output, never hex or px values other than 0.
- If a component you need doesn't exist, build it from existing ones and call it out as a "candidate component" in your reply.
- If you're not sure which token to use, check its description in Figma or the tokens file and pick the most semantic one (for helper text, `text/secondary` rather than `text/tertiary`).
