# ScholarSync UI Redesign — Session Handover

## Read First
- **decisions.md** — All design/UX/architecture decisions (the source of truth)
- **CLAUDE.md** — Points to decisions.md
- **scholarsync-prd-for-stitch.md** — PRD for Google Stitch / Gemini 3 prototyping

## Current State of localhost:4000

The prototype is a single `index.html` file served by `npx serve` on port 4000. It's a Superhuman-inspired visual redesign of ScholarSync's workspace.

### What's Built and Working
1. **Projects (formerly Dashboard)** — Google Docs-style project list. "New" button with dropdown, search bar, dual filter system (status: All/Active/Completed + type: Draft/Research/Review/Presentation). No cards, no widgets — just your work.
2. **Editor (Draft)** — Clean distraction-free writing surface. Minimal toolbar: Title, Save status, Cite (with badge), Audit, Export. Sidebar auto-collapses when entering editor. No side panels by default.
3. **Buddy panel** — Docked side panel (not overlay), pushes editor content aside. Two mode toggles (Draft/Learn) with animated circular fill effect. Gear icon for settings popover (AI Intensity: Focus/Collaborate/Accelerate). Send button with paper plane animation. Panel stays open until explicitly closed.
4. **Sidebar** — Collapsible. Three sections: Create (Draft, LaTeX, Canvas, Poster, Stage), Research (Discover, Reading Room, Pulse, Deep Research, Library, Systematic Review), Audit (Integrity Check). Hamburger menu in topbar to bring it back.
5. **Resizable panels** — Both sidebar and Buddy panel have drag handles. Min/max widths enforced.
6. **Command palette** — Cmd+K, quick actions + recent projects.
7. **Placeholder views** — All other tools have placeholder screens with descriptions.

### What's NOT Built Yet (Placeholder Only)
- Deep Research (was built earlier, simplified to placeholder in Superhuman rebuild)
- Discover (was built earlier, simplified to placeholder)
- Systematic Review (was built earlier, simplified to placeholder)
- Library (was built earlier, simplified to placeholder)
- All other tool pages (Reading Room, Pulse, LaTeX, Canvas, Poster, Stage, Audit standalone)

### Visual System (Superhuman hero-inspired)
- Background: #FFFFFF (clean white)
- Sidebar: #1E1145 (deep warm purple — Superhuman hero palette) with animated gradient orbs (violet + pink, slow drift)
- Sidebar interactions: Glassmorphism hover (backdrop-filter blur), glowing active indicator bar, press scale feedback
- Surface: #F7F5F3 (light warm gray)
- Accent: #6D28D9 (violet/purple)
- Border radius: 6px
- Borders: rgba(0,0,0,0.06) light, rgba(0,0,0,0.1) strong
- Fonts: DM Sans (UI), Source Serif 4 (headings), IBM Plex Mono (data)
- Transitions: 0.25s cubic-bezier(0.4, 0, 0.2, 1)
- Buddy icon: buddy-icon.png (orange robot illustration)

### Sidebar Icon Colors (jewel-tones for deep purple sidebar)
| Tool | Icon Shape | Hex |
|------|-----------|-----|
| Projects | 4-square grid | #E0CBFF |
| Draft/Editor | 3 stacked layers | #93C5FD |
| LaTeX | Code brackets `</>` | #CBD5E1 |
| Canvas | Artboard with pen strokes | #D8B4FE |
| Poster | Column layout + header bar | #F0ABFC |
| Stage | Screen with play triangle | #67E8F9 |
| Discover | Magnifying glass | #FCD34D |
| Reading Room | Open book | #93C5FD |
| Pulse | RSS feed icon | #FDBA74 |
| Deep Research | Magnifier with + (TBD) | #C4B5FD |
| Library | 3 upright books | #6EE7B7 |
| Systematic Review | PRISMA funnel (TBD) | #BEF264 |
| Audit | Shield with checkmark | #FCA5A5 |

## Key Design Decisions Summary

1. **Workspace model:** Content-centered, tools as capabilities. Not separate apps.
2. **Projects (formerly Dashboard):** Google Docs style — flat list of all projects across all tools, sorted by recency. Color dot = type.
3. **Sidebar:** Deep purple Superhuman-hero palette with animated gradient orbs. Glassmorphism hover, glowing active indicator. Auto-collapses in editor. User-invoked elsewhere. Three sections: Create, Research, Audit.
4. **Buddy:** Cursor-agent model. Three modes (Default, Draft, Learn). Panel pushes content, doesn't overlay. Gear menu for AI Intensity. Research happens through Buddy (no separate research button). Paper cards in chat (Option B — Cursor-style actionable cards, not chatbot text).
5. **Editor:** Distraction-free. No permanent side panels. Toolbar: Cite, Audit, Export only. No Research button (goes through Buddy).
6. **AI Intensity:** Focus/Collaborate/Accelerate. Lives in Buddy's gear popover. Changes system prompt behavior. All three are real and functional in the codebase.
7. **Cite button:** Fully functional in codebase. PubMed search, Library, DOI resolver, manual entry. Keeps in toolbar.
8. **Audit:** Editor has a limited panel. Standalone compliance page is full-featured. Sidebar "Audit > Integrity Check" should link to the full compliance page.
9. **Naming:** Buddy (AI agent), Discover (lit search), Reading Room (notebook/PDF chat), Pulse (feeds), Canvas (illustrations), Stage (presentations), Audit (integrity check), Draft (editor).
10. **Memory:** Mem0 for user-level memory + Graphiti/knowledge graph for research connections. Start with PostgreSQL + pgvector (Option A).

## Codebase Reference
The actual ScholarSync app lives at: `github.com/drshailesh88/ScholarSync`
- Next.js 15 + React 19 + Tailwind + Drizzle ORM + PostgreSQL + pgvector
- 100+ API endpoints, triple-agent screening, PRISMA with RoB2/ROBINS-I/QUADAS-2
- Network meta-analysis, feeds system, integrity checking
- AI: Claude (Anthropic) as primary, with research agent using 6 tools

## Open Decisions
- [ ] Final color refinement after Stitch/Gemini 3 prototyping
- [ ] Buddy icon — have PNG, may need SVG for production
- [ ] Compliance tool — does it merge fully with Audit or stay separate?
- [ ] Live collaboration UX (Liveblocks wired but no design)
- [ ] Onboarding flow
- [ ] Mobile responsiveness
- [ ] Dark mode (Daylight/Night toggle exists in current app)

## User Preferences (Shailesh)
- Wants world-class UX, not AI slop
- Design-first approach: discuss → decide → build
- Superhuman.com as primary visual inspiration
- Doesn't want slash commands as the primary interaction — buttons for everyone, commands as power-user shortcuts
- Values simplicity and clean interfaces over feature density
- Wants the app to feel like a productivity workspace, not a collection of tools
- PARA methodology appreciation but doesn't want it as the framework — just the "projects first" instinct
- Currently exploring Google Stitch for additional prototyping
