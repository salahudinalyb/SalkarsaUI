# CLAUDE.md

@AGENTS.md
@DESIGN.md

## Notes for Claude
- The target is a responsive, mobile-first web app built with Next.js, Tailwind v4 and shadcn/ui.
- For UI work, follow DESIGN.md section 0 (core rules), section 2 (tokens and breakpoints) and section 4 (component to shadcn mapping).
- Add shadcn components with `npx shadcn@latest add <name>`, then adjust their variants and sizes to match SalkarsaUI. Don't copy component code by hand.
- If the Figma MCP is connected, use the CSS name that Dev Mode shows for each variable.
