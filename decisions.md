# ScholarSync UI Redesign — Decision Log

## Product Identity

- **App name:** ScholarSync
- **AI Agent name:** Buddy
- **Buddy icon:** Orange robot illustration (provided by user). Simplified 20x20 version for inline use (two asymmetric eyes on orange circle).
- **Positioning:** Productivity workspace for people in the business of writing after research. Not limited to medical — all science streams.
- **Feel:** Warm power tool. Serious but alive. Not dull, not playful. Confident and energizing. Like a well-lit workshop with excellent tools already laid out.

---

## Design System

- **Fonts:** DM Sans (UI), Source Serif 4 (editorial headings), IBM Plex Mono (citations, data, code)
- **Spacing:** Generous. Respect the researcher's focus.
- **Visual direction:** Superhuman-inspired. The app extends superhuman.com's design language — specifically the hero section's deep purple warmth and ambient glow.
- **Colors:**
  - App background: `#FFFFFF` (clean white)
  - Sidebar: `#1E1145` (deep warm purple — Superhuman hero palette) with animated gradient orbs
  - Surface/inputs: `#F7F5F3` (very light warm gray)
  - Text primary: `#1C1917` (warm near-black)
  - Text secondary: `#78716C` (warm gray)
  - Text tertiary: `#A8A29E` (muted warm gray)
  - Accent: `#6D28D9` (violet/purple — Superhuman's signature)
  - Accent hover: `#5B21B6`
  - Borders: `rgba(0,0,0,0.08)` (subtle, not heavy)
- **Border radius:** `6px` (subtle rounding, Superhuman uses small radii — NOT 0px sharp, NOT large rounded)
- **Transitions:** `0.25s cubic-bezier(0.4, 0, 0.2, 1)` — Material's standard easing. Fast start, gentle landing. Nothing linear, nothing bouncy.
- Tool accent colors: each tool has a signature color (see Sidebar Icon System below)

---

## Naming Decisions

| Old Name | New Name | Rationale |
|----------|----------|-----------|
| Dashboard | **Projects** | Content-first naming — it shows your projects, not a dashboard |
| Editor/Studio | **Draft** | Action-oriented, signals academic writing |
| AI Agent | **Buddy** | Warm, memorable, chosen by user |
| Notebook (NotebookLM clone) | **Reading Room** | Not generic "notebook" or "chat with PDF" |
| Literature Search | **Discover** | Action-oriented |
| Feeds | **Pulse** | "Keep a pulse on your field" |
| Shield / Integrity Check | **Audit** | User loved "Shield" initially but chose "Audit" for clarity |
| Illustrate | **Canvas** | More evocative |
| Presentation/Slides | **Stage** | "Present on a stage" |
| Deep Research | **Deep Research** | Keep as-is, descriptive |
| Systematic Review | **Systematic Review** | Keep as-is, researchers know this term |
| Library | **Library** | Keep as-is |
| LaTeX | **LaTeX** | Keep as-is |

---

## Navigation Architecture

### Sidebar Structure (3 sections)

```
Projects (formerly Dashboard)

CREATE
  Draft
  LaTeX
  Canvas
  Poster
  Stage

RESEARCH (ordered by workflow: find → read → stay current → go deep → organize → formal review)
  Discover
  Reading Room
  Pulse
  Deep Research
  Library
  Systematic Review

AUDIT
  Integrity Check
```

### Sidebar Visual Design — Superhuman Hero Palette
- **Background:** Deep warm purple `#1E1145` — drawn directly from Superhuman's hero section.
- **Ambient gradient orbs:** Two soft radial gradients (violet + pink) that slowly drift and breathe behind the sidebar content. 12s and 15s animation cycles, moving in opposite directions. Subtle, alive, never distracting.
- **Text colors:** Labels at `rgba(255,255,255,0.55)`, section headers at `rgba(255,255,255,0.3)`, active items at `#fff`.
- **Glassmorphism hover (Key Design Decision):** When hovering a nav item, a frosted glass pill forms around it — `backdrop-filter: blur(12px) saturate(1.4)` with a top-edge highlight (`inset 0 0.5px 0 rgba(255,255,255,0.15)`) and soft shadow. The gradient orbs blur and glow *through* the glass, creating depth. This is the signature interaction feel.
- **Active state:** Glass stays on permanently. A 3px glowing indicator bar on the left edge (gradient from lavender `rgba(196,181,253,0.9)` to purple `rgba(168,85,247,0.6)` with box-shadow halo). Animates in with slight overshoot. Icon gets a luminous drop-shadow.
- **Press feedback:** `scale(0.98)` with 80ms snap on `:active`. Physical, like pressing a real button into a surface.
- **Section label hover:** When hovering within a section, its label subtly brightens from 0.3 to 0.45 opacity.
- **User avatar:** Gradient background matching sidebar palette, ring that glows on hover, separated from nav by a thin `rgba(255,255,255,0.06)` border.

### Sidebar Bottom Section

Two layers:
- **Utility icons** (above user profile): Settings gear + Help "?" — two small icon buttons, separated from user area by thin border. Icons at 35% opacity, brighten on hover. No text labels.
- **User profile area** (bottom): Avatar + name + chevron. Click to expand a panel with:
  - **Dark mode toggle** — slide switch, hidden under the name. Light is default. Dark is one click away but not in your face.
  - **Sign out** — with door/arrow icon.
- **Rationale for hiding theme toggle:** Light/dark mode is a set-once preference, not a frequent action. It doesn't deserve prime real estate as a standalone icon. Living under the user name keeps the sidebar bottom clean while remaining accessible.

### Sidebar Customization — Workspace-First Architecture

- The sidebar moved from a fixed tool catalog to a customizable workspace. Users add and remove tools to build their own sidebar.
- **Draft is the only mandatory tool.** Every user gets Draft. It cannot be removed.
- **"+ Add tool" button** at the bottom of the sidebar (above the user area). Dashed border, subtle, inviting. Opens a panel showing all available tools organized by Create/Research/Audit categories.
- **Remove a tool**: Hover on any nav item (except Projects and Draft) to reveal a small × on the right side. Click × and the item slides out with a collapse animation. The tool returns to the "Add tool" panel as available.
- **Category headers auto-show/hide**:
  - Categories (CREATE, RESEARCH, AUDIT) only appear when they have tools in them
  - CREATE label only shows if more than just Draft is present (Draft alone = no header)
  - Empty categories disappear entirely
  - When a category gains its first tool, the header appears automatically
- **Minimal state**: A user with only Draft sees: Projects, Draft, + Add tool. No headers, no categories. Cleanest possible sidebar.
- **Full state**: A researcher with all tools sees the familiar categorized sidebar with CREATE, RESEARCH, AUDIT sections.
- **Rationale**: A journalist seeing LaTeX and Systematic Review thinks "this isn't for me." A student seeing 12 tools thinks "this is too complex." By letting users build their own sidebar, every persona feels the product was made for them. The depth is discovered, not imposed.
- **Non-removable defaults**: Projects (always visible, not a tool) and Draft (the floor — everyone writes).
- **No saved workspaces**: Unlike Adobe Premiere Pro's workspace switching, ScholarSync has one sidebar per user. No naming, no switching, no workspace management UI. Just add and remove tools. Simple.
- **Onboarding implication**: During onboarding, ask what kind of work the user does. Pre-configure their sidebar with relevant tools. They can always add more or remove what they don't need.

### Sidebar Behavior
- **Auto-collapses when entering the editor (Draft) or LaTeX editor.** Writing = distraction-free.
- **Does NOT auto-collapse on other pages.**
- **Hamburger menu in topbar** to bring it back when hidden.
- **Close button (chevron)** inside sidebar to manually hide it from any page.

---

## Projects (formerly Dashboard) Design

- **Inspired by Google Docs home page.** Flat list of all projects, not a feature showcase.
- **No tool grid, no stats widgets, no activity feeds.**
- **No greeting, no hero card, no stats.** After extensive analysis of Superhuman, Notion, Linear, Google Docs, Apple Pages, Readwise, and Microsoft Word, the decision was made to keep the Projects page clean with no preamble. Reasoning:
  - Every best-in-class productivity tool shows a flat list of recent work with no preamble
  - A hero card for "most recent project" assumes the user wants to continue that project — wrong 50% of the time
  - Greetings are cosmetic and don't help the user decide what to do next
  - The warm purple sidebar with glassmorphism already provides personality — the content area doesn't need to duplicate warmth
  - A clean list respects the researcher's focus: open app, see work, click, start writing
  - The Google Docs model (flat list, filters, search) was validated as the right pattern
- **"New" button** (top-left) with dropdown of project types.
- **Search bar** to filter projects instantly.
- **List/Grid view toggle** (pill toggle, far right of filter bar):
  - **List view** (default): Rows with color dot, title, meta, timestamp. Same as Google Docs list.
  - **Grid view**: Google Docs-style cards with document preview (title + simulated text lines with fade-out gradient), color dot, truncated title, and meta. Cards hover-lift with subtle shadow. `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr))`.
  - Toggle is a segmented control (list icon = three lines, grid icon = four squares) inside a subtle pill with surface background and border. Active button gets white background + shadow.
  - Filters and search work identically in both views.
- **Combined filters:**
  - Status: `All | Active | Completed`
  - Type: `Draft | Research | Review | Presentation`
  - These combine: "Active + Research" shows only in-progress research sessions.
  - Type filters are optional toggles (click to select, click again to deselect).
- **Project list:** Each row shows color dot (type), title, contextual meta, timestamp.
- **Color dot tells you the type.** No labels needed. Users learn the colors quickly.
- **PARA-aligned:** Dashboard shows Projects (active work). Resources/Archives are in sidebar (Library, etc.).

---

## Editor (Draft) Design

- **Distraction-free by default.** No left panel, no right panel. Just a toolbar and writing surface.
- **One thin toolbar:** Document title, save status, AI intensity dropdown, Cite button (with badge), Research button, Audit button, Export button.
- **Writing surface:** 700px max-width, centered, generous padding. Source Serif for headings, DM Sans for body, 1.8 line-height.
- **AI Intensity selector** (Focus/Collaborate/Accelerate) lives in the toolbar as a dropdown. These are REAL features that change Buddy's system prompt — not cosmetic.
  - Focus: Buddy is quiet, only responds when asked
  - Collaborate: Balanced, suggestions + responses (default)
  - Accelerate: Proactive, comprehensive feedback, full rewrites

### What was REMOVED from the current editor:
- Left panel (Write/Learn toggle, Current Draft nav, References list, AI Credits) — all gone
- Right panel with 3 tabs (Chat & Learn, Research, Checks) — all gone
- These functions moved into Buddy panel or toolbar buttons

---

## LaTeX Editor Design

- **"Compile" renamed to "Preview PDF"** — reframes from developer action to user action.
- **Toolbar is ultra-minimal:** Title + Save | Preview PDF | ... (more menu)
- **The ... (more) menu** is organized into 4 sections with labels:
  - **Tools:** Files, Cite, Figures
  - **Layout:** Source / Split / Preview layout toggle
  - **Collaboration:** Comments, Track Changes, Collaborators, Version History
  - **Export:** Export PDF, Export .tex, Export .zip
- Each menu item has keyboard shortcut hints where applicable.
- All features from codebase are accessible through the more menu — nothing hidden, nothing lost.
- **Panels slide in when invoked:** Files panel from left, Cite/Comments/Track Changes/Figures/Collaboration/Version History from right (slides over the preview pane).
- **Split view is always default**, with a working drag-to-resize handle (accent color on hover, clamped 20-80%).
- **Buddy is the same global Buddy FAB** — no separate LaTeX agent panel. The floating action button works identically here as in Draft.
- **Sidebar auto-collapses when entering LaTeX** (same decision as Draft editor — writing = distraction-free).

---

## Buddy (AI Agent) Design

### Core Philosophy
Buddy is modeled after **Cursor's agent / GitHub Copilot** — an AI that lives INSIDE the workspace, not a separate tool. It is contextual, ambient, and aware of everything across all modules. Buddy is NOT a chatbot bolted onto the side. It is the intelligence layer of the workspace.

### Panel Behavior
- **Collapsible side panel** (not overlay). Docked to right edge, like Cursor's agent panel.
- **When open, editor content area shrinks** to make room. No overlap. User sees both their document and Buddy's responses simultaneously.
- **Hidden by default.** Only the floating FAB button (bottom-right, orange, robot icon) is visible.
- **Click FAB -> panel slides in from right.** FAB hides when panel is open.
- **Panel stays open until user explicitly closes it** (clicks X). Does NOT auto-close.
- **Rationale:** Unlike the sidebar (which auto-collapses in editor for distraction-free writing), Buddy is the user's active collaborator. If they opened it, they want it there. Closing it is a conscious choice.

### Panel Header — Superhuman-Style Bold Identity
- **Header layout:** `[Avatar + Name]  [Chat] [Draft] [Learn]  [Gear] [X]`
- **Avatar:** 36px, rounded square, shadow — larger and bolder than before.
- **Name:** 17px, 700 weight — confident presence.
- **Gear icon:** Material Design filled cog SVG, transparent background by default. Purple pill appears only on hover. Spins on hover.
- **Close icon:** Simple X, transparent by default. Gray pill on hover only.

### Three Modes
- **Chat mode (default):** General LLM chat. Will answer anything including fried fish recipes. Aware of all user's data across all modules. This is the base state.
- **Draft mode:** Writing copilot. Helps with writing, citations, structure. If invoked outside the editor, Buddy asks "Which draft do you want to work on?"
- **Learn mode:** Socratic teacher. Teaches through questions, tests understanding. Does not give answers — asks the user questions to build understanding.

### Mode Switching — Superhuman-Style Card Tabs
- **Three full-width card-style tabs** replacing the old small toggle buttons:
  - **Chat** (purple `#6D28D9`) — sparkle/chat bubble icon on solid purple square
  - **Draft** (teal `#0891B2`) — pen nib icon on solid teal square
  - **Learn** (green `#15803D`) — lightbulb icon on solid green square
- Each tab has a **colored icon square** (22px, border-radius 6px) — Superhuman product-logo style (solid color, white symbol).
- **Active tab:** Bold colored border + white background + shadow (pops forward like a selected card).
- **Hover animation:** Tab lifts `translateY(-1px)`, icon scales 1.12 with 2deg rotation.
- **Press:** `scale(0.98)` snap.
- **Panel background subtly shifts color per mode** (faint blue for Draft, faint green for Learn).
- **No confirmation messages when switching modes** — the active tab IS the feedback.
- **No BUDDY / DRAFT tags on messages** — the tab already indicates mode.
- **Three mode colors chosen for distinguishability** across color temperatures: purple (cool), teal (between), green (natural) — not two blues, not red next to purple.
- **Slash commands also work** as power-user shortcuts: `/draft`, `/learn`, `/default`

### Research Through Buddy (Key UX Decision)
- **No separate research button/tab in the agent panel.** User just asks Buddy to search in natural language.
- **No separate research button in the editor toolbar either.** Research is a conversation with Buddy, not a button click.
- Buddy uses existing `/api/research-agent` endpoint (same backend as Research page).
- Has access to all 6 tools: searchPubMed, searchSemanticScholar, searchOpenAlex, getPaperDetails, findSimilarPapers, savePaperToLibrary.
- **Rationale:** If the user is writing about GLP-1 and cardiovascular mortality, they can ask Buddy "find me papers on GLP-1 and CV mortality" without leaving the editor. The same search that would run on the Research page runs here. Same APIs, same results, different surface.

### Paper Cards in Chat (Option B — Chosen)
- When Buddy finds papers, show them as **inline paper cards** in the chat — not just text descriptions.
- Card shows: title, authors, journal, year, evidence level badge, citation count.
- Action buttons on each card: **Save to Library** / **Cite in Draft**.
- **Cursor analogy:** Cursor shows code blocks with syntax highlighting + Accept/Reject. Buddy shows paper cards with metadata + Save/Cite. The paper is actionable, not described.
- **Technical implementation:** `useChat` hook already provides `message.parts` array with `tool-invocation` results containing structured paper data. Currently only text parts are rendered. Option B adds a renderer that detects tool results from search tools and renders them as card components. The card component already exists on the Research page. Zero new backend code.
- **Rationale:** The difference between "the agent told me about a paper" and "the agent showed me the paper with a save button" is the difference between a chatbot and a workspace. This is where the product lives. Small decisions like this cumulate and separate quality products from AI slop.

### AI Intensity — Gear Menu (Decided)
- Focus/Collaborate/Accelerate controls live inside Buddy's **gear/settings popover**.
- **Gear icon placement:** Far right of Buddy panel header, just before the close (X) button. This follows the universal pattern (Cursor, Slack, VS Code all do this).
- **Gear popover contents:** Radio buttons for Focus / Collaborate / Accelerate with one-line descriptions.
- **Extensible:** The gear popover is where future Buddy settings will go (memory preferences, response length, etc.) without ever touching the header layout.
- **NOT in the editor toolbar.** AI Intensity only affects Buddy's chat behavior — it doesn't change the document editor itself.
- **Rationale:** Keeping intensity in a gear menu avoids clutter. Having 5 controls (2 mode toggles + 3 intensity options + close) in a 360px header would be overwhelming. The gear menu hides rarely-changed settings behind one click.

### Resizable Panels
- **Both sidebar and Buddy panel are resizable** via drag handles, like VS Code / Cursor.
- **Buddy panel:** Thin drag handle (3px) on its left edge. Min-width 280px, max-width 50% of viewport.
- **Sidebar:** Thin drag handle on its right edge. Min-width 180px, max-width 320px.
- **Cursor:** Changes to `col-resize` on hover over the handle.
- **Editor content reflows** automatically as panels resize (flex-based layout).
- **Rationale:** Users have different monitors (13" laptop vs 27" display), different tasks (quick question to Buddy vs reading a long research summary), and different preferences. Resizable panels are standard in every serious workspace tool — users expect it and notice when it's missing.

### Cross-Module Awareness
- Buddy is aware of everything across ALL modules: Library contents, draft documents, research sessions, systematic review state, Pulse feed items.
- When answering questions, Buddy can reference the user's own data: "You have 7 papers on this in your Library" or "Your draft mentions 3 mechanisms."
- **This is the moat.** This is what separates Buddy from opening ChatGPT in another tab. Context awareness across the entire workspace.

---

## Audit Design
- **Audit is its own sidebar section**, not inside the editor panels.
- **Also accessible from editor toolbar** as a button that opens an overlay/panel.
- **Not a Buddy function** — it's a separate tool.
- Shows: Overall score, AI detection %, plagiarism matches, citation issues.
- Each issue shows the flagged passage with explanation and "Fix with Buddy" option.

---

## Sidebar Icon System — Unified White Monochrome

All icons are white monochrome on the deep purple sidebar. The approach evolved from individual jewel-tone colors to a unified white system — the purple sidebar provides all the color richness, so icons don't need individual colors.

### Icon Implementation
- **SVG icons** use `#fff` fill/stroke with opacity control (60% default, 85% hover, 100% active)
- **PNG icons** use CSS `filter: brightness(0) invert(1) opacity(0.6)` to render as white
- No more individual jewel-tone colors per icon

### Icon Table

| Module | Icon | Type | Source |
|--------|------|------|--------|
| Projects | 4-square grid | SVG (white) | Original |
| Draft | Pencil on square (edit icon) | PNG | icons/edit.png |
| LaTeX | Code brackets `</>` | SVG (white) | Original |
| Canvas | Pen tool with bezier handles | PNG | icons/pen-tool.png |
| Poster | Hanging poster with mountains + text | PNG | icons/poster.png |
| Stage | Presentation/business analyst | PNG | icons/business-analyst.png |
| Discover | Magnifying glass | SVG (white) | Original |
| Reading Room | Desk lamp with books (study table) | PNG | icons/reading-room.png |
| Pulse | RSS feed icon (dot + two arcs) | SVG (white) | Lucide RSS pattern |
| Deep Research | Brain with lightbulb (creativity) | PNG | icons/creativity.png (sized 20px, opacity 0.75 to compensate for detail) |
| Library | 3 upright books | SVG (white) | Original |
| Systematic Review | PRISMA funnel (3 narrowing bars) | SVG (white) | Original |
| Audit / Integrity Check | Shield with checkmark | SVG (white) | Original |

### Icon Design Principles
- **All icons are white monochrome** — no individual colors. The purple sidebar is the color.
- **Unified opacity system:** 60% default, 85% hover, 100% active.
- **PNG icons that are too detailed at 18px** are sized to 20px with 75% base opacity (Deep Research, Stage).
- **Icon selection priority:** Use established, recognizable shapes (RSS for Pulse, pen tool for Canvas, edit icon for Draft) rather than inventing custom designs.
- **No scale-on-hover** — Icons brighten via opacity change on hover instead. Scale bouncing is amateur.
- **Active icon glow** — `filter: brightness(1.2) drop-shadow(0 0 3px rgba(196,181,253,0.3))` on the active item.

---

## Agent Memory Architecture (for future implementation)

- **"I remember you"** -> User-level memory (preferences, habits, conversation history). Recommended: Mem0 (50K stars, works with Claude, hybrid storage).
- **"I understand your research"** -> Knowledge-level memory (paper relationships, topic evolution, cross-module connections). Recommended: Graphiti by Zep (20K stars, temporal knowledge graph, MCP server).
- **Start with PostgreSQL only** (Option A): buddy_user_memories, knowledge_entities, knowledge_relations, buddy_conversations tables. User's existing pgvector stack handles this.
- **Migrate to PostgreSQL + Neo4j** (Option B) when graph queries become complex.

---

## Key Competitive Insights

- **OpenAI Prism** (launched Jan 2026): Free, multi-pane AI writing workspace for researchers. Biggest threat. ScholarSync's moat is breadth (entire research lifecycle) and unified workspace.
- **Elicit:** Query-driven pipeline, not separate tools. Good UX.
- **SciSpace:** Document-anchored, tools appear contextually on PDF.
- **Best dashboards show YOUR WORK, not THEIR PRODUCTS** (Linear, GitHub, Notion pattern).
- **Content is the anchor.** Tools appear around it, not instead of it.

---

## Technical Notes

- **Focus/Collaborate/Accelerate are REAL features.** They change the system prompt via `getDraftSystemPrompt()` in `/src/lib/ai/prompts/draft.ts`. Each mode has distinct overlay instructions affecting proactivity, response length, and feedback depth. Used in both `/api/chat` and `/api/latex/draft-chat` routes. Tested.
- **Research agent integration:** Existing `/api/research-agent` with 6 tools. Can be consumed via `useChat` from `@ai-sdk/react`. Same 3-line hook used on Research page can power the Buddy panel. No new backend code needed.
- **Tool results in chat messages:** `message.parts` array contains `tool-invocation` parts with structured paper data. Currently only text parts are rendered. Option B renders tool results as paper cards inline.

---

## Landing Page Design

- **Colors aligned with app**: Purple accent `#714CB6` → `#6D28D9` (matches app). Dark sections `#0D0D1A` → `#1E1145` (matches app sidebar). All `rgb(113,76,182)` references updated to `rgb(109,40,217)`.
- **Logo unified**: Double chevron icon replaced with "S" mark in purple rounded square — matches the app sidebar logo.
- **Module names aligned**: Literature Search → Discover, Slides → Stage, Illustrations → Canvas. All tab labels, section labels, and footer links updated.
- **Product tab icons**: Each tab now uses the same icon as the app sidebar (PNG files or matching SVGs), rendered inside colored squares with module accent colors. No duplicate colors across tabs.
- **Navigation simplified**: Stripped from [Platform, Solutions, Resources, Pricing] down to [Pricing, Blog]. Rationale: Platform/Solutions are enterprise jargon for a product without enterprise customers. Resources is vague. Training/Videos belong inside the app. Blog is the SEO engine.
- **Button anatomy unified**: Was 5 different button styles. Now 2: Primary (solid purple `#6D28D9`, 6px radius) for CTAs, and text links for secondary actions. "Get started" simplified from dark pill with lavender circle to a clean purple button with arrow.
- **Content background difference is intentional**: Landing page uses `#F2F0EB` (warmer), app uses `#FFFFFF` (cleaner). Marketing sites should feel warmer than the product — Google's marketing site looks different from Google Docs. By design.

---

## Open Decisions

- [x] Sidebar color palette — deep purple Superhuman hero palette with animated gradient orbs (decided)
- [x] Sidebar icon system — unified white monochrome with opacity control (decided, evolved from jewel-tone)
- [x] Sidebar interaction design — glassmorphism hover, glowing active indicator, press feedback (decided)
- [x] Dashboard renamed to Projects (decided)
- [x] Poster tool — placed under Create, after Canvas, before Stage
- [x] Deep Research icon — brain with lightbulb (creativity), PNG (decided)
- [x] Systematic Review icon — PRISMA funnel, SVG (decided)
- [x] Dashboard design — clean list, no greeting/hero/stats, Google Docs model (decided)
- [x] Buddy panel redesign — Superhuman-style bold identity with card tabs (decided)
- [x] Sidebar bottom section — theme toggle, settings, help icons (decided)
- [x] LaTeX editor — ultra-minimal toolbar, Preview PDF, more menu, split default (decided)
- [x] Sidebar customization — workspace-first with add/remove tools (decided)
- [x] Landing page alignment — colors, icons, nav, buttons unified with app (decided)
- [ ] Buddy icon — user provided robot illustration, needs simplified version for inline use
- [ ] Compliance tool — exists in codebase, unclear if it merges with Audit
- [ ] Live collaboration UX — Liveblocks is wired, but no design decisions made
- [ ] Onboarding flow — no decisions made
- [ ] Empty states — what first-time users see on each page
