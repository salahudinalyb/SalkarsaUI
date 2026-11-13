# DESIGN.md

This is the main reference for building UI with SalkarsaUI. It's written for developers and for AI coding tools such as Claude Code, Codex, Cursor, Copilot, v0, Lovable and Bolt.

- Platform: responsive web, mobile first (mobile web, then tablet, then desktop)
- Default stack: React or Next.js with Tailwind CSS v4 and shadcn/ui. Plain CSS works too, since every token is a CSS variable.
- Files: `tokens/tokens.css` (all tokens), `tokens/shadcn.css` (bridge for shadcn and Tailwind), `tokens/tokens.json` (DTCG)
- Figma: pages "0. SalkarsaUI Docs" (one doc frame per component), "1. Foundations" and "2. Components"
- Inventory: 51 components, 959 icons, 573 variables, 21 text styles, 12 effect styles, 4 grid styles

---

## 0. Core rules

1. Don't hard-code colors, spacing, radius or font values. Use a token, a shadcn utility (`bg-primary`) or the Tailwind 4px scale (`p-4`). If nothing fits, ask instead of making one up.
2. Use semantic tokens only, never primitives. Write `--bg-brand-default` or `bg-primary`, not `--color-blue-500` or `bg-blue-500`.
3. Pair foreground and background: `bg-primary` with `text-primary-foreground`, `--bg-warning-default` with `--text-on-warning`, `--bg-inverse` with `--text-inverse`, and `--bg-<status>-subtle` with `--text-<status>`.
4. Reuse the components in section 4. With shadcn, install the mapped shadcn component and apply the SalkarsaUI variants and sizes instead of building look-alikes.
5. Every interactive element has Default, Hover, Focus (focus-visible), Pressed/Active and Disabled states. See section 3.1.
6. Touch targets are at least 44px on touch devices. Controls are 40, 48 or 56px tall.
7. Write mobile-first CSS. Base styles are for mobile, then add `md:` and `lg:` for larger screens (section 2.3).
8. Light and dark use the same token names. Switch with `data-theme="dark"` or `class="dark"` on `<html>`. Don't pick colors in JavaScript.
9. Give icon-only buttons an accessible name (`aria-label`) and use semantic HTML (`button`, `a`, `label`, `nav`, `main`, `table`).

---

## 1. Setup

### Tailwind v4 and shadcn/ui (recommended)
```css
/* app/globals.css */
@import "tailwindcss";
@import "./tokens.css";   /* SalkarsaUI tokens */
@import "./shadcn.css";   /* points shadcn variables at SalkarsaUI */
```
Delete the color variables that shadcn generated in globals.css, since `shadcn.css` replaces them. After that, shadcn components look like SalkarsaUI:
```tsx
<Button>Simpan</Button>                       // primary
<Button variant="destructive">Hapus</Button>  // danger
<p className="text-muted-foreground">Helper</p>
```

### Plain CSS
```css
@import "./tokens.css";
.card { background: var(--surface-default); border: 1px solid var(--border-default); border-radius: var(--radius-lg); padding: var(--spacing-md); }
```

### Token names in code
Figma `a/b/c` becomes CSS `--a-b-c`, so `bg/brand/default` becomes `--bg-brand-default`. Every token has a description, in Figma or as `$description` in `tokens.json`. In Tailwind, use `bg-(--bg-subtle)` or `text-(--text-tertiary)` for tokens that don't have a shadcn alias.

---

## 2. Foundations

### 2.1 Color

**shadcn aliases and the tokens behind them**
| Tailwind class | Token | Use |
|---|---|---|
| `bg-background` / `text-foreground` | `--bg-default` / `--text-primary` | page |
| `bg-card` | `--surface-default` | cards, lists, tables |
| `bg-popover` | `--surface-raised` | menus, popovers, dialogs, calendar |
| `bg-primary` / `text-primary-foreground` | `--bg-brand-default` / `--text-on-brand` | primary action |
| `bg-secondary` | `--bg-neutral-default` | secondary button |
| `bg-accent` | `--bg-muted` | hover on ghost buttons and menu items |
| `bg-muted` / `text-muted-foreground` | `--bg-muted` / `--text-secondary` | subdued areas and skeletons / helper text |
| `bg-destructive` | `--bg-danger-default` | danger button, errors |
| `bg-success`, `bg-warning`, `bg-info` (plus `-foreground`) | the default status fills | solid badges, alerts, progress |
| `border-border`, `border-input`, `ring-ring` | `--border-default`, `--border-default`, `--border-focus` | lines, fields, focus |

**All semantic color tokens** (use as `var(--…)`):
- **Background:** `bg-default`, `bg-subtle` (hover on neutral rows and items), `bg-muted` (pressed, skeleton, progress track), `bg-emphasis`, `bg-inverse` (tooltip, toast), `bg-disabled`, `bg-overlay` (scrim), `bg-transparent`
- **Surface:** `surface-default`, `surface-raised` (with a shadow), `surface-sunken`, `surface-overlay`
- **Status fills:** `bg-{brand|success|warning|danger|info}-{default|hover|pressed|subtle|subtle-hover}`, `bg-neutral-{default|hover|pressed}`
- **Text:** `text-primary`, `text-secondary`, `text-tertiary`, `text-placeholder`, `text-disabled`, `text-inverse`, `text-on-brand`, `text-on-warning`, `text-brand`, `text-link-{default|hover|visited}`, `text-{success|warning|danger|info}`
- **Icon:** `icon-{primary|secondary|disabled|inverse|on-brand|brand|success|warning|danger|info}`
- **Border:** `border-{default|subtle|strong|emphasis|disabled|inverse|brand|focus|success|warning|danger|info}`
- **Effect:** `effect-shadow`, `effect-shadow-strong`, `effect-focus-ring`
- **Charts:** `chart-categorical-1` to `8`, for data visualization only, used in order

What the status colors mean: brand is the primary action, success is positive, warning means caution, danger is for destructive actions and errors, and info is neutral information.

### 2.2 Spacing (4px grid, same as the Tailwind default scale)
| Token | px | Tailwind | Use |
|---|---|---|---|
| `--spacing-2xs` | 4 | `1` | tight inline gaps |
| `--spacing-xs` | 8 | `2` | icon to label, chip padding |
| `--spacing-sm` | 12 | `3` | compact padding, label to field |
| **`--spacing-md`** | **16** | **`4`** | **default padding, mobile page margin** |
| `--spacing-lg` | 20 | `5` | larger containers |
| `--spacing-xl` | 24 | `6` | between groups, dialog padding, tablet margin |
| `--spacing-2xl` | 32 | `8` | between sections, desktop margin |
| `--spacing-3xl` / `4xl` | 40 / 48 | `10` / `12` | section spacing |
| `--spacing-5xl` to `8xl` | 64 / 80 / 96 / 128 | `16` / `20` / `24` / `32` | page level (hero, landing) |

Only use the Tailwind steps in this table. Skip `p-7`, `p-9` and `p-11`.

**Sizes:** icons `--size-icon-{xs 12, sm 16, md 20, lg 24, xl 32, 2xl 40}`, control heights `--size-control-{xs 32, sm 40, md 48, lg 56, xl 64}`, avatars `--size-avatar-{xs 24 up to 2xl 96}`, and `--size-touch-target-min` 44.

### 2.3 Breakpoints, grid and layout
| Breakpoint token | Min width | Tailwind | Grid (columns, gutter, margin) | Figma grid style |
|---|---|---|---|---|
| base (mobile) | 0 | no prefix | 4, 16, 16 (`--layout-grid-*`) | `grid/mobile` (frame 393) |
| `--breakpoint-sm` | 640 | `sm:` | 4, 16, 16 | none |
| `--breakpoint-md` | 768 | `md:` | 8, 16, 24 (`--layout-grid-tablet-*`) | `grid/tablet` (frame 768) |
| `--breakpoint-lg` | 1024 | `lg:` | 12, 24, 32 (`--layout-grid-desktop-*`) | `grid/desktop` |
| `--breakpoint-xl` | 1280 | `xl:` | 12, 24, 32, with capped content | `grid/desktop-1440` (frame 1440) |
| `--breakpoint-2xl` | 1536 | `2xl:` | same as xl | none |

CSS variables can't be used inside `@media`, so use the Tailwind prefixes or the numbers above.

**Layout tokens:** `--layout-container-max-width` 1280 (`max-w-7xl mx-auto`), `--layout-navbar-height` 64, `--layout-sidebar-width` 256 (`w-64`), `--layout-form-max-width` 560, `--layout-prose-max-width` 680, `--layout-dialog-width-{sm 480, md 640, lg 800}`. On mobile: `--layout-app-bar-height` 56 and `--layout-bottom-nav-height` 64. For safe areas, use `env(safe-area-inset-*)`.

**How elements change across breakpoints**
| Element | Changes at | From | To |
|---|---|---|---|
| App bar | `lg` | App bar | Navbar |
| Bottom navigation | `md` | Bottom nav | Sidebar (or Navbar links) |
| Dialog | `md` | Drawer or Size=Mobile | centered Dialog, Size=sm/md/lg |
| Bottom sheet, Action sheet | `md` | Sheet | Dialog, Popover or Menu |
| Form primary button | `sm` | full width | auto width, right-aligned |
| Table | `md` | list items or cards (or horizontal scroll) | Table |
| Page title | `lg` | `heading-sm` | `heading-lg` |
| Hero title | `lg` | `heading-xl` | `display-md` |
| Section title | `lg` | `title-lg` | `heading-xs` |

Body text stays at 16px on every screen size, which also stops iOS from zooming into inputs.

### 2.4 Shape
**Radius:** `--radius-{none 0, 2xs 2, xs 4, sm 6, md 8, lg 12, xl 16, 2xl 20, 3xl 24, 4xl 32, full 9999}`.

In Tailwind (through shadcn.css): `rounded-sm` is 4, `rounded-md` is 6, `rounded-lg` is 8 (buttons, inputs), `rounded-xl` is 12 (cards, tables, popovers), dialogs use `rounded-(--radius-xl)` (16), and `rounded-full` for pills.

**Border width:** thin 1 (default), thick 2 (focus, selected), heavy 4. **Opacity:** disabled 0.4, overlay 0.6.

### 2.5 Typography
Use the `.text-*` classes from tokens.css.

| Class | Font | Size / line height |
|---|---|---|
| `.text-display-lg/md/sm` | Inter Bold | 48/56, 44/52, 40/48 |
| `.text-heading-xl/lg/md/sm/xs` | Inter SemiBold | 36/44, 32/40, 28/36, 24/32, 20/28 |
| `.text-title-lg/md/sm` | Inter SemiBold | 18/28, 16/24, 14/20 |
| `.text-body-lg/md/sm/xs` | Inter Regular | 18/28, 16/24, 14/20, 12/16 |
| `.text-label-lg/md/sm` | Inter Medium | 16/24, 14/20, 12/16 |
| `.text-caption` | Inter Regular | 12/16 |
| `.text-overline` | Inter SemiBold, uppercase | 11/16 |
| `.text-code` | Roboto Mono | 14/20 |

When to use each: display for hero and landing sections, heading for page and section titles, title for card, list and dialog titles, body for content (`body-md` by default, `body-sm` in tables), label for text inside controls, and caption for metadata.

Load Inter (400, 500, 600, 700) and Roboto Mono (400), for example with `next/font/google`.

### 2.6 Elevation and focus
`--shadow-xs`, `--shadow-sm` (card), `--shadow-md` (menu, popover, calendar), `--shadow-lg` (toast), `--shadow-xl` (dialog, sheet), `--shadow-2xl`, `--shadow-inner`, and `--blur-background-{sm|md|lg}` for frosted sticky headers.

**Focus:** buttons, chips, tabs, links, nav items, pagination and calendar days use `--focus-ring-offset` (a 2px gap plus a 2px `--border-focus` ring), which is `focus-visible:ring-2 ring-ring ring-offset-2 ring-offset-background` in Tailwind. Text fields (Input, Select, Textarea) use `--focus-ring` with a `--border-focus` border, which is `focus-visible:border-ring focus-visible:ring-3 ring-ring/50`.

### 2.7 Icons
Figma has 959 icons named `Icon/<kebab-name>`, each with `Style = Regular | Filled`. Regular is the default. Use Filled for active or selected states, like the active Nav item or Sidebar item. Until the icon package ships, use the closest **lucide-react** icon with the same name (`arrow-right` becomes `<ArrowRight />`). Sizes: 16 (`size-4`) inline and small, 20 (`size-5`) by default, 24 (`size-6`) in navigation. Color them with `currentColor` or an `icon-*` token.

---

## 3. From Figma to code

### 3.1 States (all interactive components)
| State | Figma variant | Code | Visual |
|---|---|---|---|
| Hover | `State=Hover` | `hover:` (pointer devices only) | the `*-hover` fill, `bg-subtle` on neutral items, or `border-strong` on fields |
| Focus | `State=Focus` | `focus-visible:` | `--focus-ring-offset` on controls, `--focus-ring` on fields |
| Pressed | `State=Pressed` | `active:` | the `*-pressed` fill, or `bg-muted` |
| Active, Selected, Current | `State=Active/Selected/Current` | a prop plus `aria-current`, `aria-selected` or `data-[state=active]` | brand subtle fill or brand border |
| Open | `State=Open` | `data-[state=open]:` | the focus look, plus the menu or popover |
| Error | `State=Error` | `aria-invalid` | `border-danger` with a `text-danger` helper |
| Loading | `State=Loading` | `loading` prop and `aria-busy` | spinner, label stays visible |
| Disabled | `State=Disabled` | `disabled` | the `*-disabled` tokens |

### 3.2 Props
- Text properties become strings: `Label` becomes `children` or `label`, `Helper` becomes `helperText`, `Error text` becomes `errorText`.
- `Show X` booleans mean "render X when it's provided". Don't add `showX` props.
- `Leading icon` and its swap become `leadingIcon`, or the icon goes in as the first child of a shadcn Button.
- Slot properties (`Prefix`, `Suffix`, `Body`, `Footer`, `Items`, `Actions`, `Links`, `Navigation`, `Rows`, `Content`) become `ReactNode` or children.
- `Variant`, `Size`, `Status`, `Color`, `Style`, `Type`, `Align`, `Sort` and `Shape` become lowercase string unions.
- Interaction states (Hover, Focus, Pressed, Open) are handled in CSS, not props.

---

## 4. Components (51) and their shadcn/ui equivalents

**Actions**
| SalkarsaUI | shadcn/ui | Notes |
|---|---|---|
| **Button** | `button` | Variants: primary (shadcn `default`), secondary, outline, ghost, danger (shadcn `destructive`). Heights: sm 40 (`h-10`), md 48 (`h-12`), lg 56 (`h-14`), with `rounded-lg` and `text-label-md`. Content order is icon, prefix, label, suffix, icon. While loading, the label stays visible ("Memproses…"). Use one primary button per view, and `w-full sm:w-auto` in forms. |
| **Icon button** | `button size="icon"` | Ghost (default), secondary or primary, 40 or 48. `aria-label` is required. |
| **Chip** | `toggle` or `badge` with a remove button | Sizes 32 and 36, `rounded-full`. Selected chips use the brand subtle fill, a brand border and a check. |

**Inputs**
| SalkarsaUI | shadcn/ui | Notes |
|---|---|---|
| **Input** | `input` with `label` (and `input-group`) | Heights 40, 48 or 56, `rounded-lg`, `bg-card`, `border-input`. Hover uses `border-strong`, focus uses the ring border plus ring, and errors use `aria-invalid`. Content order is icon, prefix addon, value, suffix, trailing icon. |
| **Input addon** | `input-group` addon | For things like +62 ▾, Rp, https:// or kg. |
| **Textarea** | `textarea` | Label, helper text and a counter. |
| **Select** | `select`, or `drawer` on mobile for long lists | Same anatomy as Input, plus a chevron. |
| **Checkbox**, **Radio**, **Switch** | `checkbox`, `radio-group`, `switch` | Standard behavior. Radios always sit in a group. |
| **Segmented control** and **Segment** | `toggle-group` or `tabs` (pill) | Segments share the width equally. |
| **Date picker** | `popover` with `calendar` (date-picker) | An Input (value like "20 Nov 2026", calendar icon) with the Calendar 4px below it. Typing dd/mm/yyyy also works. On mobile, use a sheet or a native `type="date"`. |
| **Calendar** and **Calendar day** | `calendar` (react-day-picker) | Weeks start on Monday, with day labels Sen to Min. Days are 40×40, with states Today (brand border), Selected (brand fill), In range, Outside and Disabled. |

**Feedback**
| SalkarsaUI | shadcn/ui | Notes |
|---|---|---|
| **Alert** | `alert` | Info, success, warning or danger (shadcn `destructive`), with a subtle background and status-colored text and icon. |
| **Toast** | `sonner` | Bottom center on mobile, bottom right on desktop. Closes after about 4 seconds unless it has an action. |
| **Tooltip** | `tooltip` | `bg-inverse` with `text-inverse`. Keep the text short, and never make a tooltip the only way to reach information. |
| **Badge** | `badge` | Any color in subtle or solid style, with an optional dot. |
| **Spinner** | `spinner` or `Loader2 animate-spin` | 16 to 48px, for waits of unknown length. |
| **Skeleton** | `skeleton` | `bg-muted animate-pulse`, in text, heading, circle, rectangle or list item shapes, with `aria-busy`. |
| **Progress** | `progress` | sm 4px or md 8px, in brand, success, warning or danger, with `role="progressbar"`. |

**Navigation**
| SalkarsaUI | shadcn/ui | Notes |
|---|---|---|
| **Navbar** and **Navbar link** | a custom `<header><nav>` (or `navigation-menu`) | Desktop only (lg and up), 64 tall and sticky. Contains the logo, 3 to 6 links, and actions (search, notifications, one primary button, avatar). Link states are Default, Hover, Focus and Active (`aria-current`). |
| **Sidebar** and **Sidebar item** | `sidebar` | md and up, 256 wide, `bg-subtle`. Groups have an overline label, and items have a 20px icon, a label and a count badge. The active item uses the brand subtle fill and a Filled icon. |
| **Breadcrumb** and its item | `breadcrumb` | md and up, separated by chevron-right icons. The last item is the current page and isn't a link. |
| **Pagination** and its item | `pagination` | Previous, up to 7 pages including ellipses, then Next. The current page has a border. On mobile, show icon-only buttons and "2 / 10". |
| **App bar** | a custom `<header>` | Mobile only (below lg). |
| **Bottom navigation** and **Nav item** | a custom `<nav>` | Mobile only (below md), 3 to 5 items, 64 tall plus the safe area. |
| **Tabs** and **Tab** | `tabs` | Underline or pill style. Hover uses `text-primary`. |
| **Menu** and **Menu item** | `dropdown-menu` | Items are at least 44 tall on touch and 36 with a pointer. Hover uses `bg-subtle`, and the keyboard highlight looks the same as hover. |

**Containers**
| SalkarsaUI | shadcn/ui | Notes |
|---|---|---|
| **Card** | `card` | `bg-card`, `rounded-xl`, with a border or `--shadow-sm`. |
| **Dialog** | `dialog`, or `drawer` on mobile | Size=Mobile is 328 with buttons split 50/50. sm is 480, md is 640 and lg is 800, with buttons aligned right. |
| **Popover** | `popover` | 320 wide, `bg-popover`, a border, `rounded-xl` and `--shadow-md`, with a header and content. Esc or a click outside closes it. On mobile, use a sheet. |
| **Bottom sheet** | `drawer` or `sheet side="bottom"` | Mobile only. Top radius 24, max height 90dvh. |
| **Action sheet** and its item | `drawer` on mobile, `dropdown-menu` on desktop | Destructive items use the danger color. |
| **List item** | `item` or custom | States Default, Hover (`bg-subtle`) and Pressed (`bg-muted`), with a minimum height of 56. |

**Data display**
| SalkarsaUI | shadcn/ui | Notes |
|---|---|---|
| **Table** (with Table row, Table cell and Table header cell) | `table` with the TanStack `data-table` | The container uses `bg-card`, a border and `rounded-xl`. The header is 40 tall (`label-md text-muted-foreground`) and sortable with `aria-sort`. Rows are at least 56 tall with `border-subtle`, use `bg-subtle` on hover and `bg-(--bg-brand-subtle)` when selected. Cell types: text, two lines, user, badge, number (right-aligned), checkbox and actions (⋯). The footer shows a summary like "Menampilkan 1-5 dari 48" next to Pagination. On mobile, use a list or cards. |
| **Avatar** | `avatar` | Sizes 24, 32, 40, 48, 64 and 96. Falls back from photo to initials to icon. |

**System (Figma mockups only, never build these):** Status bar and Home indicator.

---

## 5. Patterns
- **Page shell:** on mobile, a sticky App bar, the content (`px-4 py-6`) and a Bottom nav. On tablet, a Sidebar and the content (`px-6`). On desktop, a Navbar or Sidebar with the content in `max-w-7xl mx-auto px-8`.
- **List page:** Breadcrumb, then the page title with the primary action, a toolbar (search, filters), a Table on desktop or List items on mobile, and Pagination.
- **Form:** labels above fields, `gap-4` between fields and `gap-6` between groups. Validate on blur. The submit button is full width on mobile (sticky, above the safe area) and right-aligned on desktop. Forms are at most 560 wide.
- **Loading:** use `loading` on buttons, a Skeleton for content and Progress when the length is known. Avoid full-screen spinners.
- **Empty state:** an illustration, a `heading-sm` title, `body-md text-muted-foreground` text and one primary button, with `py-12`.
- **Destructive action:** a danger button plus a confirmation (a small Dialog on desktop, a Drawer on mobile).
- **Copy:** Bahasa Indonesia, sentence case, with short verbs ("Simpan", "Batal", "Memproses…"). Dates look like "20 Nov 2026" and amounts like "Rp1.250.000".

## 6. Accessibility
- Use the token pairs in section 2.1, which meet AA contrast. Don't invent new pairs.
- Show a visible `:focus-visible` style on everything interactive (section 2.6).
- Keep touch targets at least 44px on mobile, and make everything reachable by keyboard on desktop (Tab order, Esc closes overlays, arrow keys in menus, calendars and tabs).
- Set `aria-current="page"` on the active navigation item (Navbar link, Sidebar item, Breadcrumb, Pagination).
- Don't rely on color alone. Pair status colors with an icon or text.
- Respect `prefers-reduced-motion` and `prefers-color-scheme`.

## 7. Notes for AI tools
- Read sections 0, 2, 3 and 4 before generating UI. Only use tokens, shadcn aliases and the Tailwind steps listed above. No hex values, no arbitrary px values (`p-[13px]`), and no palette classes (`bg-blue-500`).
- Build the mobile layout first, then add `md:` and `lg:`, and swap components using the table in section 2.3.
- Implement every state in section 3.1.
- If a component is missing, compose it from section 4 or restyle the shadcn default with tokens, and call it out as "candidate component: <name>".
- If you're not sure which token to use, read its `$description` in `tokens/tokens.json`.
