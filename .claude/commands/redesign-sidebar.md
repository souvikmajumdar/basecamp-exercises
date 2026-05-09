# Redesign UI: Vertical Sidebar Navigation

Redesign the Vue 3 app's UI into a modern SaaS-style interface by replacing the top navigation bar with a fixed vertical sidebar on the left.

## Before starting

If anything about the current app structure is unclear, ask before proceeding. Do not assume.

## What to change

### App.vue — shell layout only

Replace the `.top-nav` block with a fixed left sidebar. The sidebar should contain:
- **Top**: Logo / app name
- **Middle**: Nav links (one per route, with active state)
- **Bottom**: User profile area or utility links (settings, profile, etc.)

Adjust `.main-content` to offset from the sidebar (e.g. `margin-left: 240px`). Do not change any other global styles — cards, tables, badges, color tokens, typography all stay the same.

### Design constraints
- Reuse existing color tokens: `#0f172a` (sidebar bg), `#f8fafc` (page bg), `#2563eb` (active link), `#e2e8f0` (borders)
- Sidebar width: 240px fixed
- Nav links: full-width, left-padded, with active highlight using blue tint `#eff6ff` and `#2563eb` text + a 3px left accent border
- Sidebar should be `position: fixed; top: 0; left: 0; height: 100vh; overflow-y: auto`
- No top nav bar remains — remove it entirely
- Keep `z-index` above page content

## Rules

- MANDATORY: Delegate ALL `.vue` file edits to the `vue-expert` subagent (per project CLAUDE.md)
- Do not change any view files (`views/*.vue`) or component files — only `App.vue`
- Do not alter existing global CSS classes (`.card`, `.badge`, `.stat-card`, tables, etc.)
- After editing, start the dev server and verify all routes render correctly at `http://localhost:3000`
- Check that the sidebar is visible and functional on: Dashboard, Inventory, Orders, Spending, Demand, Reports, Backlog

## Verification checklist
- [ ] Sidebar renders at correct width with logo and all nav links
- [ ] Active route is highlighted in sidebar
- [ ] Page content does not overlap the sidebar
- [ ] All 7 views load without layout breakage
- [ ] No top nav bar visible
- [ ] Existing cards, tables, and badges are visually unchanged
