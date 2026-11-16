# Changelog

Notable changes to SalkarsaUI are listed here. The format follows [Keep a Changelog](https://keepachangelog.com) and versions follow [SemVer](https://semver.org).

## [1.2.0] - 2026-11-16
### Added
- `SKILL.md`, which turns the repo into an Agent Skill. Install it once with `git clone` into `~/.claude/skills/salkarsaui` (or `~/.agents/skills/salkarsaui`) and update it with `git pull`.

### Changed
- AGENTS.md and the Cursor and Copilot rules no longer repeat exact sizes and spacing steps. They point to DESIGN.md, which is now the only place those values are written.

## [1.1.0] - 2026-11-13
SalkarsaUI now targets responsive web (mobile web, tablet and desktop) and ships a theme for shadcn/ui and Tailwind CSS v4.

### Added
- 18 components, for 51 in total: Date picker, Calendar, Calendar day, Skeleton, Progress, Navbar, Navbar link, Sidebar, Sidebar item, Breadcrumb, Breadcrumb item, Pagination, Pagination item, Popover, Table, Table row, Table cell and Table header cell.
- Dialog sizes (Mobile, sm, md, lg) and List item states (Default, Hover, Pressed).
- Hover and Focus states on Button, Icon button, Chip, Menu item, Tab, Input, Select, Textarea and all new navigation components.
- Breakpoints sm 640, md 768, lg 1024, xl 1280 and 2xl 1536, with 4, 8 and 12 column grids.
- Layout tokens: container 1280, navbar 64, sidebar 256, form 560, prose 680, and dialog widths 480, 640 and 800. There are now 573 variables in total.
- Effect style `focus/ring-offset` (12 effect styles in total) and 4 grid styles.
- `tokens/shadcn.css`, which points shadcn/ui variables at SalkarsaUI tokens. Dark mode now works with `data-theme="dark"` or `.dark`.
- `tokens/tokens.json` now also includes the effect styles (shadows, focus rings and background blurs).

### Changed
- `title/sm` is now 14/20, so it no longer matches `title/md`. All other token values from 1.0.0 are unchanged.
- DESIGN.md, AGENTS.md, CLAUDE.md, the Cursor and Copilot rules and llms.txt were rewritten for web and shadcn/ui.
- Docs and token, component and text style descriptions were reworded to read more plainly.

## [1.0.0] - 2026-11-13
### Added
- 33 components: Button, Icon button, Chip, Input, Input addon, Textarea, Select, Checkbox, Radio, Switch, Segmented control, Segment, Alert, Toast, Tooltip, Badge, Spinner, App bar, Bottom navigation, Nav item, Tabs, Tab, Menu, Menu item, Card, Dialog, Bottom sheet, Action sheet, Action sheet item, List item, Avatar, Status bar, Home indicator.
- 959 icons in Regular and Filled styles.
- 552 tokens across Primitives, Color (light), Color Dark, Spacing, Shape and Typography, each with code syntax for Web, Android and iOS and a short usage note.
- 21 text styles and 11 effect styles.
- DESIGN.md, AGENTS.md, CLAUDE.md, Cursor and Copilot rules, tokens.css and tokens.json.
- MIT license.
