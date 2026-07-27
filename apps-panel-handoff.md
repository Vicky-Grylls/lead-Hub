# Handoff Spec: Apps / Integrations in Customize Panels

## Overview

Extend the existing "Customize Panels" right sidebar modal to support third-party app integrations. Users can discover and install apps from an in-modal gallery, then toggle them on/off in their sidebar — exactly like built-in panels. Three screens cover the full flow: the enhanced panel modal, the app gallery browser, and the post-install confirmation state.

**Figma file:** `vZPHSrp8ZWk6d4ywBlpkcL`
Frames: `Customize Panels — Enhanced` | `App Gallery` | `Customize Panels — Post Install`

---

## Design Tokens

| Token | Value | Usage |
|---|---|---|
| `color-surface` | `#FFFFFF` | Modal background, row fills |
| `color-surface-subtle` | `#F9FAFB` | Section label backgrounds |
| `color-surface-input` | `#F3F4F6` | Search bar, close/back buttons |
| `color-border` | `#EFEFEF` | Horizontal dividers |
| `color-border-card` | `#E5E7EB` | App card borders |
| `color-text-primary` | `#111827` | Row labels, card names |
| `color-text-secondary` | `#9CA3AF` | Category labels, placeholders |
| `color-text-muted` | `#6B7280` | Footer, dismiss icons |
| `color-accent-blue` | `#3B82F6` | Browse button, Manage link |
| `color-accent-blue-light` | `#EFF6FF` | Browse button fill |
| `color-accent-blue-border` | `#BFDBFE` | Browse button border |
| `color-toggle-on` | `#1A1A1A` | Active toggle pill |
| `color-toggle-off` | `#D1D5DB` | Inactive toggle pill |
| `color-warning` | `#F59E0B` | Auth-required label, accent stripe |
| `color-warning-light` | `#FFFBEB` | New app row highlight fill |
| `color-warning-badge` | `#FEF3C7` | "NEW" badge background |
| `color-success` | `#10B981` | Post-install toast icon |
| `color-success-light` | `#ECFDF5` | Post-install toast background |
| `color-success-border` | `#A7F3D0` | Post-install toast bottom border |
| `color-installed` | `#16A34A` | "✓ Installed" text |
| `color-installed-bg` | `#F0FDF4` | "✓ Installed" button fill |
| `color-installed-border` | `#BBF7D0` | "✓ Installed" button border |
| `radius-modal` | `16px` | Outer modal corner radius |
| `radius-card` | `12px` | App gallery card radius |
| `radius-icon` | `8–10px` | App icon badge radius |
| `radius-chip` | `15px` | Category chip, toggle pill |
| `radius-button` | `6–10px` | Action buttons |
| `shadow-modal` | `0 8px 32px -4px rgba(0,0,0,0.14)` | Modal drop shadow |

---

## Screen 1 — Customize Panels (Enhanced)

### Layout

- Modal width: `300px` fixed
- Modal height: auto (content-driven)
- Corner radius: `16px`
- Clips content: yes
- Drop shadow: `0 8px 32px -4px rgba(0,0,0,0.14)`

### Structure (top to bottom)

| Section | Height | Notes |
|---|---|---|
| Header | `60px` | Title + close button |
| Divider | `1px` | `#EFEFEF` |
| "PANELS" section label | `34px` | Background `#F9FAFB` |
| Panel row × 5 | `52px` each | Built-in panels |
| Divider | `1px` | |
| "APPS" section label | `34px` | Includes "Manage" link (right-aligned) |
| App row (installed) | `60px` each | Per installed app |
| Browse Apps button | `60px` | Wrapper + button |
| Divider | `1px` | |
| Reset to Default footer | `48px` | Centered |

### Header

- Padding: `16px` left, `14px` right
- Title: `Inter Semi Bold 16px`, `#111827`
- Close button: `28×28px` circle, `#F3F4F6` fill, `✕` glyph `11px` `#6B7280`

### Section Labels

- Font: `Inter Semi Bold 10px`, letter-spacing `1.2px`, `#9CA3AF`
- Background: `#F9FAFB`
- Padding: `16px` horizontal
- "Manage" link (APPS section only): `Inter Medium 11px`, `#3B82F6`, right-aligned

### Panel Rows (built-in panels)

- Height: `52px`
- Padding: `14px` horizontal
- Gap between elements: `10px`
- Drag handle: 6-dot 2×3 grid, `3px` dots, `#C4C9D4`, `3px` gaps
- Label: `Inter Regular 15px`, `#111827`, flex-grow
- Toggle: see Toggle component below

### App Rows (installed apps)

- Height: `60px`
- Same padding/gap as panel rows
- App icon: `32×32px`, `border-radius: 8px`, coloured background, white `Bold 10px` initials
- Name: `Inter Medium 14px`, `#111827`
- Category: `Inter Regular 11px`, `#9CA3AF`
- Toggle: same as panel rows

#### Auth-Required Variant

When an installed app has not been authenticated:

- Category text replaced with: `⚠  Connect account to enable`
- Category text color: `#F59E0B`
- Toggle: visible but `opacity: 0.5`, non-interactive, pointer cursor: `not-allowed`
- Tooltip on hover: `"Connect your [App] account in Settings to enable this panel"`

### Toggle Component

- Pill size: `44×26px`, `border-radius: 13px`
- Knob: `22×22px` white circle
- On state: pill `#1A1A1A`, knob `x: 20px`
- Off state: pill `#D1D5DB`, knob `x: 2px`
- Transition: `background 150ms ease, transform 150ms ease`
- Knob always `y: 2px`

### Browse Apps Button

- Container height: `60px`, full width, padding `14px`
- Button: `272px` wide, `42px` tall, `border-radius: 10px`
- Fill: `#EFF6FF`, border: `1px solid #BFDBFE`
- Contents: `+` glyph (16px) + "Browse Apps" label (13px Medium), both `#3B82F6`
- Gap between glyph and label: `6px`
- On click: open App Gallery (slide-in or replace panel content)

### Reset to Default Footer

- Height: `48px`, centered
- Text: `↺  Reset to Default`, `Inter Medium 13px`, `#6B7280`
- Restores only the PANELS section toggles. Does not uninstall apps.

---

## Screen 2 — App Gallery

### Layout

- Same modal container as Screen 1 (`300px`, `border-radius: 16px`, same shadow)
- This is a **second view** inside the same modal — not a new modal. Animate as a horizontal slide-in from the right (`translateX(100%) → 0`, `250ms ease-in-out`).

### Structure

| Section | Height |
|---|---|
| Header (with back arrow) | `60px` |
| Divider | `1px` |
| Search bar | `56px` |
| Category chips | `48px` |
| Divider | `1px` |
| App grid rows (3 rows × 2 cards) | `170px` each |
| Bottom padding | `8px` |

### Header

- Back button: `28×28px` circle, `#F3F4F6`, `←` glyph `13px` `#374151`
- Clicking back returns to Screen 1 (slide out to right)
- Title: "App Gallery", `Inter Semi Bold 16px`
- Close button: same as Screen 1

### Search Bar

- Container height: `56px`, padding `14px`
- Input: `272px` wide, `38px` tall, `border-radius: 10px`, fill `#F3F4F6`
- Padding inside: `12px`
- Search icon: `⌕` glyph, `16px`, `#9CA3AF`
- Placeholder: "Search apps...", `Inter Regular 13px`, `#9CA3AF`
- On focus: border `1px solid #3B82F6`

### Category Chips

- Container height: `48px`, padding `14px`, `6px` gap between chips
- Overflow: horizontally scrollable (hide scrollbar)
- Chip height: `30px`, `border-radius: 15px`, padding `12px` horizontal
- Font: `Inter Medium 12px`
- **Active chip:** fill `#1A1A1A`, text `#FFFFFF`
- **Inactive chip:** fill `#F3F4F6`, text `#6B7280`
- Categories: All | CRM | Finance | Comms | Support (extensible)

### App Cards (2-column grid)

- Grid row height: `170px` (card `154px` + `8px` top/bottom padding)
- Card width: `130px`
- Card gap: `12px`
- Grid padding: `14px` horizontal
- Card: `border-radius: 12px`, `border: 1px solid #E5E7EB`, fill `#FFFFFF`

#### Card Contents (absolute positioned within 130×154px)

| Element | Position | Size | Style |
|---|---|---|---|
| App icon | `x:12, y:14` | `40×40px`, `border-radius: 10px` | Brand color bg, white Bold 12px initials |
| App name | `x:12, y:62` | — | `Inter Semi Bold 13px`, `#111827` |
| Category | `x:12, y:80` | — | `Inter Regular 11px`, `#9CA3AF` |
| Action button | `x:12, y:112` | `106×28px`, `border-radius: 6px` | See below |

#### Card Action Button States

| State | Fill | Border | Text |
|---|---|---|---|
| Install | `#111827` | none | "Install" — `Inter Medium 11px` white |
| Installed | `#F0FDF4` | `1px #BBF7D0` | "✓ Installed" — `Inter Medium 11px` `#16A34A` |
| Installing | `#111827` opacity 60% | none | Spinner + "Installing..." |

On **Install click:**
1. Button enters "Installing..." loading state (spinner, 60% opacity)
2. On success: button becomes "✓ Installed"
3. App is added to top of APPS list in Screen 1 with "NEW" badge
4. When user navigates back to Screen 1, post-install toast is shown

---

## Screen 3 — Post-Install Confirmation

### Layout

Same modal as Screen 1, with two additions at the top.

### Success Toast (top of modal, above header)

- Height: `44px`, full width
- Fill: `#ECFDF5`
- Bottom border only: `1px solid #A7F3D0`
- Check circle: `20×20px`, `border-radius: 10px`, fill `#10B981`, white `✓` glyph `10px Bold`
- Message: `"[App name] added to your sidebar"` — `Inter Medium 13px`, `#065F46`, flex-grow
- Dismiss: `✕` glyph `11px`, `#6B7280`
- Auto-dismiss after `4000ms` with fade-out (`opacity 300ms ease`)

### Newly Installed App Row (highlighted)

- Fill: `#FFFBEB` (replaces default `#FFFFFF`)
- Left accent stripe: `3px` wide, full height, fill `#F59E0B`
- "NEW" badge: `34×18px`, `border-radius: 4px`, fill `#FEF3C7`, text "NEW" `9px Bold #D97706`
- Badge appears between the label stack and the toggle
- Highlight fades away after `6000ms` (transition `background 600ms ease`)
- NEW badge disappears on next modal open

### App Row Sort Order

Newly installed apps appear **first** in the APPS list. After the first session, apps sort by most recently toggled.

---

## Interactions & Animations

| Trigger | Element | Animation | Duration | Easing |
|---|---|---|---|---|
| Click "Browse Apps" | Gallery panel | Slide in from right | `250ms` | `ease-in-out` |
| Click "←" back | Gallery panel | Slide out to right | `250ms` | `ease-in-out` |
| Toggle on/off | Toggle knob | Translate X + fill change | `150ms` | `ease` |
| Install success | Action button | Cross-fade to Installed state | `200ms` | `ease` |
| Post-install toast | Toast | Fade in from top (`-8px → 0`) | `250ms` | `ease-out` |
| Toast auto-dismiss | Toast | Fade out | `300ms` | `ease` |
| New app row highlight | Row background | Fade to white after 6s | `600ms` | `ease` |
| Drag handle (reorder) | Any row | Standard drag-and-drop, ghost preview | — | — |

---

## States Reference

### APPS Section States

| State | Description |
|---|---|
| No apps installed | Hide the APPS section entirely. "Browse Apps" button still visible below PANELS. |
| 1+ apps installed | Show APPS section label + rows + Browse button |
| App installed, not authed | Show row with amber warning text, disabled toggle |
| App installed, authed, on | Show row with active toggle |
| App installed, authed, off | Show row with inactive toggle |

### Empty State (No Apps Installed)

Show only the Browse Apps button below the PANELS divider. No "APPS" section label. Add supporting copy: `"Extend your sidebar with integrations"` in `#9CA3AF 12px` above the button.

---

## Accessibility

- **Modal role:** `role="dialog"`, `aria-modal="true"`, `aria-label="Customize Panels"`
- **Focus trap:** focus cycles within modal when open
- **Focus order:** Header close → Panel rows → App rows → Browse Apps → Reset → (back to close)
- **Toggle:** `role="switch"`, `aria-checked="true/false"`, `aria-label="[Panel name]"`
- **Disabled toggle (auth):** `aria-disabled="true"`, `aria-describedby` pointing to warning text
- **Drag handles:** `aria-label="Drag to reorder [name]"`, support keyboard reorder with `Space` to grab, `↑↓` to move, `Space/Enter` to drop
- **App cards:** `role="button"`, keyboard focusable
- **Toast:** `role="status"`, `aria-live="polite"`
- **Minimum touch target:** `44×44px` for all interactive elements (toggles are 44×26px pill but hit area should be padded to 44×44px)
- **Color contrast:** All text meets WCAG AA (4.5:1 minimum)

---

## Notes for Engineering

1. **Apps data model** — Each app needs: `id`, `name`, `category`, `iconColor`, `initials`, `isInstalled`, `isAuthenticated`, `isSidebarEnabled`. The sidebar toggle should be disabled (not hidden) when `isAuthenticated = false`.

2. **Ordering** — The panel list (both PANELS and APPS sections) should persist order via `localStorage` or user preferences API. Default order = installation order.

3. **Gallery is lazy-loaded** — Don't fetch the app catalog until the user clicks "Browse Apps" for the first time. Cache for the session.

4. **App icon strategy** — Phase 1: use initials + brand hex. Phase 2: support `iconUrl` for real logos (SVG preferred, fallback to initials if URL fails).

5. **"Manage" link** — Links to a full-page integrations settings view. Out of scope for this modal — just needs a route target.

6. **Max visible apps** — If a user installs more than 5 apps, the APPS section becomes scrollable (max height `300px`, overflow scroll). Built-in panels are always fully visible.
