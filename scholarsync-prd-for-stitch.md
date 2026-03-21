# ScholarSync — Product Requirements Document for UI Prototype

## What is ScholarSync?

ScholarSync is an AI-powered research workspace for scientists, researchers, and academics. It is a single unified platform where researchers discover papers, read and analyze them, write manuscripts, create presentations, generate scientific illustrations, run systematic reviews, and check their work for integrity — all without leaving the app.

The AI assistant is called **Buddy**. Buddy lives as a collapsible side panel (like Cursor IDE's agent panel) and is available on every screen. Buddy is context-aware — it knows what's in the user's library, their drafts, their research sessions, and their ongoing reviews.

---

## App Structure

### Navigation

The app has a **left sidebar** for navigation and a **floating Buddy panel** on the right (opened on demand).

**Left sidebar sections:**

```
Dashboard

CREATE
  Draft
  LaTeX
  Canvas
  Poster
  Stage

RESEARCH
  Discover
  Reading Room
  Pulse
  Deep Research
  Library
  Systematic Review

AUDIT
  Integrity Check
```

The sidebar auto-collapses when the user enters the editor (Draft) for distraction-free writing. A hamburger menu icon in the top bar brings it back.

**Top bar:** Hamburger menu (left), breadcrumb showing current page (center-left), search/command palette trigger with ⌘K shortcut (right).

---

## Pages & Screens

### 1. Dashboard

**Purpose:** Show the user their work. Not a feature showcase — a file browser like Google Docs.

**Layout:**
- Top row: "New" button (outlined style), search input, filter chips
- Filter chips in two groups:
  - Status: `All | Active | Completed`
  - Type: `Draft | Research | Review | Presentation`
- Below filters: flat list of all projects sorted by recency

**Each project row shows:**
- Small colored dot (indicating project type — each type has a distinct color)
- Project title
- Contextual metadata (e.g., "3,240 words · 12 citations · Methods in progress" for a draft, or "Screening · 35 of 47 papers reviewed" for a review)
- Timestamp ("2h ago", "1d ago", etc.)

**Sample project list items:**
1. "Impact of SGLT2 Inhibitors on Cardiac Remodeling in HFpEF" — 3,240 words · 12 citations · Methods in progress — 2h ago
2. "SGLT2 mechanisms in HFpEF — multi-perspective synthesis" — Completed · 28 papers · 4 perspectives — 6h ago
3. "AI Diagnostics in Radiology — Meta-analysis" — Screening · 35 of 47 papers reviewed — 1d ago
4. "NEJM Submission Results — Conference Talk" — 18 slides · Defense prep not run — 3d ago
5. "Oxidative stress pathways in SGLT2i cardioprotection" — Completed · 19 papers · 3 perspectives — 4d ago
6. "Letter to Editor: Response to Martinez-Rios commentary" — 890 words · 4 citations · Final review — 5d ago
7. "Cardiac MRI Protocol — Supplementary Methods" — LaTeX · 2 files · Last compiled successfully — 1w ago
8. "GLP-1 Agonists in Obesity — Systematic Review" — Completed · 62 papers · Published in Lancet — 3w ago

**"New" button dropdown options:**
- New Draft
- New LaTeX Project
- New Deep Research
- New Systematic Review
- New Presentation
- New Illustration

---

### 2. Draft (Editor)

**Purpose:** Distraction-free academic writing with AI assistance available on demand.

**Layout:**
- Thin toolbar at top: document title (editable), save status ("Saved 2 min ago"), Cite button (with citation count badge), Audit button, Export button
- Below toolbar: clean writing surface, centered content column (max 700px), generous padding
- No left panel, no right panel by default — just the document
- Buddy panel opens from the right on demand (resizable, pushes content aside)

**Toolbar buttons:**
- **Cite** — opens citation dialog (search PubMed, browse library, resolve DOI/PMID, manual entry)
- **Audit** — runs integrity check (AI detection, plagiarism, citation verification)
- **Export** — PDF or Word download

**Sample document content:**
- Title: "Impact of SGLT2 Inhibitors on Cardiac Remodeling in Heart Failure with Preserved Ejection Fraction"
- Subtitle metadata: "Journal of Cardiac Failure · Original Article · Draft"
- Sections: Introduction, Methods, Statistical Analysis
- Inline citations like [Chen et al., 2023], [Anker et al., 2021], [Solomon et al., 2022]
- Academic medical writing style

**The sidebar auto-collapses when entering this page.**

---

### 3. Buddy Panel (appears on every page)

**Purpose:** AI assistant that lives beside your work. Like Cursor IDE's agent panel.

**Trigger:** Floating button in bottom-right corner showing the Buddy robot icon. Click to open the panel. Panel slides in from the right and pushes page content aside (not an overlay).

**Panel layout:**
- Header row: Buddy icon + "Buddy" name (left), Draft mode toggle button + Learn mode toggle button (center), gear icon + close icon (right)
- Chat messages area (scrollable)
- Input bar at bottom with text input and animated Send button

**Mode toggles:**
- Two buttons: `[ Draft ]` and `[ Learn ]`
- Neither selected = default mode (general AI chat, answers anything)
- Click Draft = Draft mode (writing copilot — citations, structure, research)
- Click Learn = Learn mode (Socratic teacher — asks questions, tests understanding)
- Click an active mode again = deselect, back to default
- Draft button fills with deep blue when active
- Learn button fills with green when active
- Animated fill effect on hover (color reveals from bottom-left corner)

**Gear icon popover (settings):**
- AI Intensity setting with three radio options:
  - Focus — "Responds only when asked"
  - Collaborate — "Balanced suggestions and feedback" (default)
  - Accelerate — "Proactive rewrites and comprehensive feedback"

**Sample chat messages:**
- Buddy: "Hey Shailesh. Ask me anything — or click Draft / Learn to switch modes."
- User: "Find me papers on GLP-1 and cardiovascular mortality"
- Buddy: "I found 23 papers across PubMed and Semantic Scholar. Here are the most relevant:" [followed by paper cards with title, authors, journal, year, citation count, and Save to Library / Cite in Draft buttons]

**The panel is resizable** — drag handle on its left edge. Min width 280px, max 50% of viewport.

---

### 4. Deep Research

**Purpose:** Multi-perspective AI research synthesis. User asks a question, Buddy searches across multiple databases and synthesizes findings.

**Layout:**
- Left: Research input area (textarea for research question, depth selector, start button)
- Right: Evidence panel showing papers found

**Depth options (selectable cards):**
- Quick Scan — ~15 papers, 2 min
- Standard — ~30 papers, 5 min (default)
- Deep Dive — ~60 papers, 12 min
- Exhaustive — ~100 papers, 25 min

**After starting:**
- Left panel shows live progress feed (what Buddy is doing in real time):
  - "Generated 4 perspectives: Mechanistic pathways, Clinical outcomes, Biomarker evidence, Safety profile"
  - "Round 1: Searched PubMed + Semantic Scholar — found 34 papers"
  - "Round 2: Citation traversal — 12 additional papers via backward citations"
  - "Extracting data from 28 full-text papers..."
  - "Synthesizing findings across perspectives..."
- Right panel shows paper cards as they're found (title, authors, journal, year, citation count, evidence level badge, Save to Library button)

---

### 5. Discover

**Purpose:** Search 282M+ papers across PubMed, Semantic Scholar, OpenAlex, ClinicalTrials.gov.

**Layout:**
- Large search input at top
- Filter chips below: All Sources, PubMed, Semantic Scholar, ClinicalTrials.gov, RCTs only, date range, Open Access
- Results list below filters

**Each result shows:**
- Evidence level badge (I, II, III)
- Paper title
- Authors
- One-line AI-generated relevance note from Buddy (e.g., "Landmark RCT showing 21% risk reduction in CV death/HF hospitalization with empagliflozin in HFpEF. n=5,988.")
- Journal name, year, citation count (right side)

---

### 6. Reading Room

**Purpose:** Upload multiple papers and chat with Buddy across all of them (like NotebookLM). Every insight is traceable to a specific page in a specific paper.

**Layout:**
- Left: list of uploaded/added papers
- Center: conversation with Buddy (with source markers [1][2][3] linking to papers)
- Right (on demand): PDF viewer showing the exact page when you click a source marker

---

### 7. Pulse

**Purpose:** Stay current with new research. Literature feeds and alerts.

**Layout:**
- Left: subscription list (feed names, unread counts)
- Center: article feed (title, journal, date, one-line summary per article)
- Keyboard navigable: J/K to move, S to save, Space to preview

---

### 8. Library

**Purpose:** Your paper collection. Every paper you've ever saved, instantly searchable.

**Layout:**
- Search bar at top
- Smart view filter chips: All Papers, Cited in Draft, This Week, [project-specific views]
- Table view: Title, Authors, Journal, Year, Citations columns
- Sortable columns, clickable rows

---

### 9. LaTeX

**Purpose:** Full LaTeX editor with live PDF preview and AI assistance.

**Layout:** Split pane — code editor on left, live PDF preview on right.

---

### 10. Canvas

**Purpose:** Scientific figures, diagrams, and illustrations. Buddy generates, user refines. Journal-ready export.

---

### 11. Poster

**Purpose:** Academic conference posters. AI-generated layouts from research content. Print-ready export.

---

### 12. Stage

**Purpose:** Presentations from research. Supports conference talks, thesis defense, journal clubs, grand rounds.

**Layout:**
- Slide thumbnail strip (left)
- Current slide canvas (center, 16:9)
- Content/notes panel (right, collapsible)

---

### 13. Systematic Review

**Purpose:** PRISMA 2020-compliant systematic review workflow.

**Layout:**
- Horizontal pipeline bar at top showing stages: Protocol → Search → Import → Screening → Extraction → Risk of Bias → Meta-Analysis → PRISMA → Report
- Current stage highlighted, completed stages show checkmarks
- Below: the active stage's interface

**Screening stage (most complex):**
- One paper at a time, full view
- Paper title, authors, journal, year
- Abstract text
- Buddy's recommendation with triple-agent consensus: "Recommend: Include — Matches population criteria, RCT design. Triple-agent consensus: 3/3 Include · Confidence: High"
- Action buttons: Include, Exclude, Maybe, Skip
- Progress indicator: "35 of 47 papers reviewed"

---

### 14. Audit — Integrity Check

**Purpose:** Check your work before submission. Plagiarism detection, AI content detection, citation verification, writing quality analysis.

**Layout:**
- Score overview at top (Original %, AI-detected %, Citation issues count)
- Collapsible sections:
  - AI Detection — per-paragraph flags with suggestions
  - Plagiarism — matched excerpts with source links
  - Citation Audit — verified vs unverified references, issues list
  - Writing Quality — readability grade, passive voice count, suggestions
- Action buttons: "Rephrase with Buddy" on flagged passages

---

## Buddy Robot Icon

Buddy's icon is an orange circle with a friendly white robot — asymmetric blue eyes (one light blue, one dark blue), rectangular body, mechanical claw arms. It appears as:
- A floating circular button (bottom-right of screen) when Buddy panel is closed
- A small avatar in the Buddy panel header

---

## Interaction Patterns

- **Command palette (⌘K):** Available everywhere. Search projects, jump to any page, quick actions.
- **Sidebar:** Collapsible. Auto-hides in editor. Hamburger to bring back.
- **Buddy panel:** Collapsible right panel. Stays open until explicitly closed. Resizable via drag handle.
- **Both sidebar and Buddy panel are resizable** with drag handles.
- **Filters combine:** On dashboard, status + type filters work together (e.g., "Active" + "Draft" shows only in-progress drafts).

---

## Key Copy / Text

**App name:** ScholarSync
**Tagline:** Built for deep work
**AI Assistant name:** Buddy

**Navigation labels:** Dashboard, Draft, LaTeX, Canvas, Poster, Stage, Discover, Reading Room, Pulse, Deep Research, Library, Systematic Review, Integrity Check

**Dashboard heading:** (none — just the project list, Google Docs style)
**Editor save status:** "Saved 2 min ago"
**Buddy greeting:** "Hey Shailesh. Ask me anything — or click Draft / Learn to switch modes."
**Send button label:** "Send" (with animated paper plane icon)

**Mode button labels:** "Draft" and "Learn"
**Gear popover heading:** "AI Intensity"
**Intensity options:** "Focus", "Collaborate", "Accelerate"

**Filter labels:**
- Status: "All", "Active", "Completed"
- Type: "Draft", "Research", "Review", "Presentation"

**New button dropdown:** "New Draft", "New LaTeX Project", "New Deep Research", "New Systematic Review", "New Presentation", "New Illustration"
