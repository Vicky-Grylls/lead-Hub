# Session Summary: Prelude Job Details Page Prototype

## Who
**Vignesh** (Vignesh@zuper.co) — Product Designer at **Zuper**, a Field Service Management (FSM) platform. Working on the **"Prelude Design Revamp"** — a new design language for Zuper's web application.

## What Was Built
A **single-file, pixel-perfect working HTML prototype** of a **Job Details page** for Zuper's FSM application.

**File:** `/sessions/kind-sweet-brown/mnt/Prelude New Job Details Page - Web/job-details.html` (856 lines)

## Tech Stack
- Single HTML file with inline React 18 (CDN + Babel transpilation)
- Tailwind CSS v3 via CDN with custom config for Inter font
- Lucide icons via CDN with custom React `I` component using refs
- Inter font from Google Fonts (400, 500, 600, 700)
- Custom `Dropdown` component (NO native `<select>` elements — user explicitly forbids iOS/OS native pickers)

## Design Tokens (from Figma)
- Shell background: `#f8f5f0`
- Active/selected: `#e4d9c8`
- Content background: `#fdfdfc`
- Borders: `#e5e5e5` / `#e5e7eb`
- Text high: `#262626`
- Text medium: `#737373`
- AI gradient: `linear-gradient(169deg, #faf5ff, #eff6ff)`
- AI border: `#e9d4ff`

## Page Architecture

```
App
├── TopNavBar (48px, #f8f5f0 bg)
│   ├── Zuper logo (orange gradient "Z")
│   ├── Active tab: "Job -#JN-245678" (highlighted with #e4d9c8)
│   ├── 5 Invoice tabs (closable)
│   ├── "All Tabs" dropdown
│   └── Right icons: smile, search, calendar, layout-grid, bell + avatar "RG"
│
├── Main Content Area (flex row)
│   ├── Sidebar (72px wide, #f8f5f0 bg, with text labels)
│   │   ├── Home, Sense (sparkles icon), divider
│   │   ├── Work Orders (active), CRM, Accounting, Dispatch, Reports, More
│   │   └── Settings at bottom
│   │
│   ├── Content Area (flex column)
│   │   ├── ModuleHeader (44px, breadcrumb + 1/30 pagination)
│   │   │   └── "Jobs > #JN-245- Roof Replacement - Telnet" + up/down nav
│   │   │
│   │   └── Three-Column Layout (flex row)
│   │       ├── LeftPane (330px, border-r)
│   │       │   ├── Cover Image (140px, Unsplash rooftop solar panels)
│   │       │   ├── Title: "Roof Replacement - Telnet"
│   │       │   ├── Address with map-pin icon
│   │       │   ├── Action buttons: Update Status, calendar, plus, bookmark, more
│   │       │   ├── Details section (expand/collapse)
│   │       │   │   ├── Base: Status (New badge), Priority (High red badge), Assignees (JD + AS + +2), Sales Rep (dashed add), Customer, Due Date, Lead Source
│   │       │   │   └── Expanded: Start/End Time, Category, Parent Job (link), Job Type, Created by
│   │       │   └── Upcoming Appointments card (date badge + title + avatar)
│   │       │
│   │       ├── CenterPane (flex-1, #fdfdfc bg)
│   │       │   ├── Tab Bar: Overview | Notes (2) | Tasks (19) | Quotes & Invoices | +6 More
│   │       │   ├── Overview Tab:
│   │       │   │   ├── Highlights (4 metric cards: Due Date, Job Value, Tasks, Job Profit)
│   │       │   │   ├── AI Job Summary (gradient card with ask-a-question, thumbs, copy)
│   │       │   │   ├── Job Description card
│   │       │   │   ├── Addresses (Service + Billing + SVG map with pin)
│   │       │   │   ├── Tasks Preview (4 tasks with status icons, time, subtask pills)
│   │       │   │   └── Custom Fields (Remarks, Additional info, Package instructions)
│   │       │   ├── Notes Tab: Empty state with "Add Note" CTA
│   │       │   ├── Tasks Tab: Filter dropdowns (Type, User) + Create button + 10 task rows
│   │       │   └── Quotes Tab: Empty state with "Create" CTA
│   │       │
│   │       └── RightPaneArea (330px panel + 46px mini sidebar)
│   │           ├── Mini Sidebar (5 icon buttons, panel-right-close at bottom)
│   │           │   ├── clock → Status History
│   │           │   ├── link → Associations
│   │           │   ├── phone → Zuper Connect
│   │           │   ├── message-circle → Chats
│   │           │   └── sparkles → Sense AI
│   │           │
│   │           ├── StatusHistoryPanel (REDESIGNED in latest iteration):
│   │           │   ├── Header with sort icon
│   │           │   ├── "Update status..." dropdown at top
│   │           │   ├── Timeline with colored dots + connector lines
│   │           │   ├── Expand/collapse cards with:
│   │           │   │   - Status name + CURRENT badge + duration-in-status (monospace)
│   │           │   │   - Avatar + user name + date/time
│   │           │   │   - Duration bar (proportional visual)
│   │           │   │   - Expanded: description, quote blockquote, travel pill, checklist pill
│   │           │   └── Statuses: On Going (current, blue), Scheduled (purple), New (teal)
│   │           │
│   │           ├── AssociationsPanel:
│   │           │   ├── User/Team Assigned (5) — collapsible with avatar card
│   │           │   ├── Timelog Summary — Labour/Break/Travel times
│   │           │   ├── Property — card with badge, date, type, priority
│   │           │   ├── Organization — "Maven Roofing" card
│   │           │   ├── Hubspot, Child Jobs, Material Requests (collapsed)
│   │           │   └── Customers — "Richard Mathew" with phone/mail/link actions
│   │           │
│   │           ├── ZuperConnectPanel:
│   │           │   ├── Header with contact name "Charles" + "Sales" badge
│   │           │   ├── Missed call (red), text messages, outgoing blue bubble
│   │           │   ├── Completed call card with audio player + reactions
│   │           │   ├── Tags (Spam, Marketing)
│   │           │   ├── AI Call Summary (gradient card with transcript link)
│   │           │   └── Bot auto-reply message
│   │           │
│   │           ├── ChatsPanel:
│   │           │   ├── Sub-tabs: Messages | Files | Pinned
│   │           │   ├── Message thread (outgoing blue, incoming gray)
│   │           │   ├── Image attachments (placeholder blocks)
│   │           │   └── Report button
│   │           │
│   │           └── SensePanel:
│   │               ├── Empty state with sparkles icon
│   │               ├── "Ask Sense about this job" prompt
│   │               └── Input field with send button
```

## Key Components

### Icon Component (`I`)
```jsx
function I({name,size=14,className='',color,sw}){
  const r=useRef(null);
  useEffect(()=>{if(!r.current)return;r.current.innerHTML='';const e=document.createElement('i');e.setAttribute('data-lucide',name);if(sw)e.setAttribute('data-lucide-stroke-width',sw);r.current.appendChild(e);lucide.createIcons({nodes:[e]});},[name,sw]);
  return <span ref={r} className={`inline-flex items-center justify-center shrink-0 [&>svg]:w-full [&>svg]:h-full ${className}`} style={{width:size,height:size,color}}/>;
}
```

### Dropdown Component (Custom — NO native selects)
```jsx
function Dropdown({label,options,width='auto'}){
  // Uses click-outside detection, supports colored dots in options
  // Options can be strings or {label, dot} objects
}
```

## Figma References
- **All Screens page:** node `5557-66490` (contains Default Layout, Left Pane 4 variants, Right Pane 4 variants, Center Pane 6 variants, Sense Window)
- **Components page:** node `4879-23988`
- Individual nodes fetched: `5557:79995` (Center Pane), `5557:79996` (Status History)

## Source Materials
- Screen recording video: uploaded as `.mov` file (17 frames extracted to `/sessions/kind-sweet-brown/frames/`)
- Cover image: User provided a rooftop solar panel image → mapped to Unsplash `photo-1613665813446-82a78c468a1d`

## Design Decisions & User Feedback

### Explicit User Requirements
1. **NO native iOS/OS select pickers** — must use custom Tailwind dropdown menus
2. **Pixel-perfect UI** matching Figma designs
3. **Consistent icons** — Lucide icon set throughout
4. **Compact fonts** — user felt fonts were "a bit big" in early iterations, asked to make them smaller
5. **Cover image** — must be a real rooftop image (solar panels)
6. **Sense icon** — must use `sparkles` icon in sidebar
7. **All interactions** — every panel, tab, expand/collapse, empty state must work

### Status History Redesign (Latest Work)
The original status history had issues:
- Rotated "10 mins" text on timeline connectors was visually broken
- Checkmark circles on every node had no visual distinction
- Cards were verbose with flat information hierarchy
- No sense of time-in-status duration

**Redesigned with:**
- Time-in-status as primary signal (monospace duration badge)
- Proportional duration bars for visual scanning
- Progressive disclosure (expand/collapse cards)
- Current status visually distinct (ping animation, CURRENT badge, tinted bg)
- Clean timeline rail with colored dots (no checkmarks)
- Metadata as pill badges (travel, checklist) on expand
- Inspired by Linear's activity feeds and Intercom's timeline patterns

**User's question about this redesign:** "What is the best way to represent the status history?" — This was an open design exploration question, not just a bug fix. The user wants to evaluate what provides the most value. They haven't yet confirmed whether this redesign direction is correct.

### Issues Fixed During Session
1. Left pane missing right border → added `border-r` with `#e5e5e5`
2. Cover image not loading → replaced with Unsplash rooftop solar panels URL
3. (User started a 3rd point but it was cut off)

## What's Pending / Open Questions
1. **User hasn't confirmed the Status History redesign** — they asked "what could give value" which is exploratory. They need to review the new version and may want changes.
2. **User's 3rd feedback point was cut off** — they listed "1. border, 2. cover image, 3." but never finished point 3.
3. **The prototype is functional** but may need further refinements after user review.

## User Profile
- **Role:** Product Designer at Zuper (FSM company)
- **Design Philosophy:** Look beyond conventional FSM UI trends, draw inspiration from Huly.io, Intercom/Fin.ai, Otter.ai/Rilla.com
- **Preferences:** Flow-first approach, compact & clean UI, custom components over native, pixel-perfect to Figma
- **Working Style:** Shares screenshots for feedback, asks design-thinking questions, iterates quickly
