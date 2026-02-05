# Navigation System - Product Requirements Document

## Document Information

| Field | Value |
|-------|-------|
| **Feature Module** | Navigation System |
| **Document Owner** | Senior Product Manager |
| **Created Date** | 2026-02-05 |
| **Status** | Draft for Review |
| **Version** | 1.0 |

---

## Table of Contents

1. [Feature Overview](#1-feature-overview)
2. [User Stories & Acceptance Criteria](#2-user-stories--acceptance-criteria)
3. [Functional Requirements](#3-functional-requirements)
4. [Non-Functional Requirements](#4-non-functional-requirements)
5. [Content & UX Copy Requirements](#5-content--ux-copy-requirements)
6. [Dependencies & Constraints](#6-dependencies--constraints)
7. [Rollout & Change Management Strategy](#7-rollout--change-management-strategy)
8. [Handoff Notes](#8-handoff-notes)

---

## 1. Feature Overview

### 1.1 Feature Name

**PMM Navigation System** - Core application navigation and wayfinding interface

### 1.2 Problem Statement

Manufacturing software users need to navigate between multiple complex modules (Inventory, Production, BOMs, Cost Lists, Sites, Reports, Configuration) efficiently and confidently. Without a clear, persistent, and well-structured navigation system, users experience:

- **Lost time** searching for features across disconnected screens
- **Reduced productivity** from unclear navigation paths and mental context switching
- **Training burden** from non-intuitive information architecture
- **Missed critical alerts** (low stock, production delays, cost variances) buried in the interface
- **Reduced adoption** of advanced features due to poor discoverability
- **Accessibility barriers** for keyboard-only users and screen reader users

PMM manages mission-critical manufacturing data. Users must be able to navigate to the right module instantly without cognitive overhead.

### 1.3 Target Users

| User Persona | Navigation Needs |
|--------------|------------------|
| **Manufacturing Engineer** | Quick access to Dashboard, Inventario, Reportes to monitor production |
| **Production Planner** | Frequent navigation between Dashboard, Inventario, and Alertas |
| **Cost Accountant** | Regular access to Reportes and Configuración for cost analysis |
| **Inventory Manager** | Primary focus on Inventario, Categorías, and Alertas (low stock warnings) |
| **Operations Manager** | Dashboard-centric view with quick access to all modules for oversight |
| **Warehouse Staff** | Simplified access to Inventario and Alertas (mobile/tablet contexts) |
| **Finance Manager** | Dashboard and Reportes for high-level KPIs |
| **System Administrator** | Heavy use of Configuración with occasional visits to all modules |

### 1.4 Business Value

#### Risk Reduction
- **Reduced navigation errors**: Clear active state indicators prevent users from working in wrong module
- **Faster incident response**: Alert badge on navigation ensures critical issues are addressed immediately
- **Audit compliance**: Navigation history supports traceability of user actions

#### Operational Efficiency
- **Time savings**: Average 30-60 seconds saved per module switch (15-30 switches/day = 7.5-30 min/day/user)
- **Reduced training time**: Intuitive navigation reduces onboarding from 2 weeks to 1 week
- **Cognitive load reduction**: Persistent navigation eliminates "where am I?" confusion

#### Scalability
- **Future module support**: Sidebar structure accommodates new PMM modules without redesign
- **Multi-site operations**: Navigation scales from single-plant to enterprise deployments
- **Role-based access**: Foundation for showing/hiding modules based on user permissions

#### User Experience
- **Increased feature adoption**: Discoverability of Reportes and Configuración improves by 40%
- **User satisfaction**: Clear navigation directly correlates with perceived system quality
- **Accessibility**: Keyboard navigation and screen reader support expands user base

### 1.5 Success Metrics

| Category | Metric | Target | Measurement Method |
|----------|--------|--------|-------------------|
| **Efficiency** | Average time to navigate to target module | < 3 seconds | Analytics tracking (click to page load) |
| **Efficiency** | Navigation-related support tickets | Reduce by 60% | Support ticket analysis (baseline vs 3 months post-launch) |
| **Adoption** | % of users accessing Reportes module weekly | Increase from 25% to 50% | User activity logs |
| **Adoption** | % of users accessing Configuración monthly | Increase from 15% to 35% | User activity logs |
| **Accuracy** | Navigation errors (clicking wrong module) | < 2% of navigation events | Error tracking / undo patterns |
| **Alert Response** | Time from alert creation to user acknowledgment | Reduce by 40% | Alert timestamp analysis |
| **Accessibility** | Keyboard-only navigation success rate | 100% | Accessibility audit |
| **Accessibility** | Screen reader compatibility score | WCAG 2.1 AA compliance | Accessibility testing tools |
| **User Satisfaction** | Navigation ease rating (1-5 scale) | ≥ 4.2 | Quarterly user survey |
| **Performance** | Navigation interaction response time | < 100ms | Frontend performance monitoring |

---

## 2. User Stories & Acceptance Criteria

### US-NAV-001: Primary Module Navigation

**As a** Manufacturing Engineer
**I want** to navigate to any major PMM module (Dashboard, Inventario, Categorías, Alertas, Reportes, Configuración) with a single click
**So that** I can access the features I need immediately without searching through nested menus

**Acceptance Criteria:**
- Left sidebar displays all six primary navigation items with icons and labels
- Each navigation item is clickable and navigates to the corresponding module
- Active module is visually highlighted (orange background as shown in screenshot)
- Navigation persists across all pages within the application
- Clicking a navigation item loads the target module within 3 seconds (on typical network conditions)
- Clicking the currently active navigation item does not cause page reload or navigation (no-op)

**Priority:** Critical
**User Impact:** High

---

### US-NAV-002: Visual Active State Indication

**As an** Inventory Manager
**I want** to see which module I'm currently in at all times
**So that** I don't accidentally perform actions in the wrong context (e.g., viewing wrong inventory, editing wrong configuration)

**Acceptance Criteria:**
- Currently active navigation item displays orange background (#F97316 or similar)
- Active item text remains white for high contrast against orange background
- Active item icon is white to match text color
- Inactive items have white text on dark blue background (#1E3A5F or similar)
- Active state updates immediately when navigating to a different module
- Only one navigation item can be active at a time
- Active state is visually distinct and recognizable at a glance (within 1 second of page load)

**Priority:** Critical
**User Impact:** High

---

### US-NAV-003: Sidebar Collapse/Expand

**As a** Warehouse Staff member using a tablet
**I want** to collapse the navigation sidebar to a compact icon-only view
**So that** I can maximize screen space for operational data entry while still having quick access to navigation

**Acceptance Criteria:**
- Collapse button is visible at the bottom of the sidebar (chevron icon pointing left)
- Clicking collapse button transitions sidebar to icon-only mode (width ~64px)
- In collapsed mode, only module icons are visible (no text labels)
- Hovering over a collapsed menu item shows a tooltip with the module name
- Expand button (chevron pointing right) is visible when sidebar is collapsed
- Clicking expand button restores full sidebar with icons and text
- Collapse/expand state persists across page navigations within the same session
- Transition animation is smooth (200-300ms) and does not disrupt content layout
- Logo area adapts in collapsed mode (shows icon only or compact version)

**Priority:** High
**User Impact:** Medium

---

### US-NAV-004: Notification Badge

**As a** Production Planner
**I want** to see a notification count badge on the top bar
**So that** I am immediately aware of critical alerts (low stock, production delays) that require my attention

**Acceptance Criteria:**
- Notification icon (bell) is visible in the top-right area of the top bar
- Badge displays unread notification count (e.g., "3") as shown in screenshot
- Badge uses high-visibility color (red #EF4444 or similar) to draw attention
- Badge is circular and positioned on the top-right of the bell icon
- Clicking the notification icon opens a notification panel or navigates to Alertas module
- Badge count updates in real-time when new notifications arrive (within 10 seconds)
- Badge disappears when notification count is zero
- Badge count displays "9+" for counts greater than 9 to maintain compact size
- Notification icon is accessible via keyboard (Tab navigation, Enter to activate)

**Priority:** Critical
**User Impact:** High

---

### US-NAV-005: User Profile Access

**As a** System Administrator
**I want** to access my user profile and account settings from the top bar
**So that** I can manage my preferences, change password, and log out without navigating away from my current work

**Acceptance Criteria:**
- User avatar/profile icon is visible in the top-right corner of the top bar
- Clicking the user icon opens a dropdown menu with options:
  - View Profile
  - Account Settings
  - Help & Documentation
  - Log Out
- Dropdown menu is positioned below the user icon and aligned to the right edge
- User icon displays current user's initials or profile photo
- Profile menu is accessible via keyboard (Tab to icon, Enter to open, Arrow keys to navigate menu)
- Clicking outside the dropdown menu closes it
- Log Out option requires confirmation before signing user out
- Profile icon is distinguishable from notification icon (different icon, adequate spacing)

**Priority:** High
**User Impact:** Medium

---

### US-NAV-006: Application Branding & Identity

**As an** Operations Manager
**I want** to see the PMM logo and application name clearly displayed in the navigation
**So that** I can quickly identify which system I'm working in (especially when managing multiple applications or browser tabs)

**Acceptance Criteria:**
- PMM logo (orange icon with factory/mill graphic) is displayed at the top of the sidebar
- "Pennant Mill Manager" text is displayed below or beside the logo in expanded sidebar mode
- Logo and text use brand colors (orange #F97316 for icon, white text)
- Logo area is clickable and navigates to Dashboard (home page)
- Application name "Pennant Mill Manager" is also displayed in the top bar for reinforcement
- Logo and branding are visible in both expanded and collapsed sidebar states (collapsed shows icon only)
- Logo area has adequate padding and visual separation from navigation items

**Priority:** Medium
**User Impact:** Low

---

### US-NAV-007: Keyboard Navigation

**As a** Power User (Cost Accountant)
**I want** to navigate between modules using keyboard shortcuts
**So that** I can work more efficiently without switching between keyboard and mouse

**Acceptance Criteria:**
- Tab key moves focus through navigation items in logical order (top to bottom)
- Enter or Space key activates the focused navigation item
- Focus indicator is clearly visible (outline or highlight on focused item)
- Keyboard shortcuts are available for common modules:
  - `Alt+D` or `Ctrl+1`: Dashboard
  - `Alt+I` or `Ctrl+2`: Inventario
  - `Alt+C` or `Ctrl+3`: Categorías
  - `Alt+A` or `Ctrl+4`: Alertas
  - `Alt+R` or `Ctrl+5`: Reportes
  - `Alt+S` or `Ctrl+6`: Configuración
- Keyboard shortcuts are documented in Help section
- Escape key closes any open dropdown menus (profile, notifications)
- Focus returns to main content area after navigation completes
- Keyboard navigation works in both expanded and collapsed sidebar states

**Priority:** High
**User Impact:** Medium

---

### US-NAV-008: Mobile/Tablet Responsive Navigation

**As a** Warehouse Staff member using a mobile device
**I want** the navigation to adapt to smaller screens
**So that** I can access all PMM modules on my phone or tablet while on the warehouse floor

**Acceptance Criteria:**
- On screens < 768px width, sidebar collapses to a hamburger menu icon
- Hamburger menu icon is positioned in the top-left corner of the top bar
- Clicking hamburger menu opens sidebar as an overlay (slides in from left)
- Overlay sidebar displays full navigation items with icons and text
- Clicking outside the overlay or on a navigation item closes the sidebar
- Close button (X) is visible in the top-right of the overlay sidebar
- Sidebar overlay does not block access to top bar (notifications, profile)
- Touch targets for navigation items are minimum 44x44px for accessibility
- Active state indication remains visible in mobile view
- Swipe gesture (left-to-right) can also open the sidebar on touch devices

**Priority:** High
**User Impact:** High (for warehouse and field users)

---

### US-NAV-009: Screen Reader Accessibility

**As a** visually impaired Finance Manager using a screen reader
**I want** the navigation to be fully accessible with assistive technology
**So that** I can navigate PMM independently and efficiently

**Acceptance Criteria:**
- All navigation items have descriptive ARIA labels (e.g., "Navigate to Dashboard")
- Active navigation item is announced as "current page" or "selected"
- Sidebar collapse/expand button has clear label ("Collapse navigation" / "Expand navigation")
- Notification badge count is announced (e.g., "3 unread notifications")
- Navigation landmark is properly marked with `<nav>` element and `role="navigation"`
- Focus order is logical and matches visual layout
- Icon-only elements (in collapsed mode) have text alternatives
- Screen reader announces page title changes when navigating to new module
- Skip navigation link is provided to jump directly to main content
- No keyboard traps exist in navigation interactions

**Priority:** High
**User Impact:** High (for accessibility compliance and inclusive design)

---

### US-NAV-010: Navigation Performance

**As a** Manufacturing Engineer
**I want** navigation interactions to feel instant and responsive
**So that** I can work fluidly without frustrating delays when switching between modules

**Acceptance Criteria:**
- Clicking a navigation item provides immediate visual feedback (< 100ms)
- Active state change occurs before page navigation begins
- Sidebar collapse/expand animation completes within 300ms
- Navigation does not block or freeze the UI during page transitions
- Loading indicators are shown if module takes > 1 second to load
- Navigation remains interactive during page loads (no disabled state)
- No visual jank or layout shifts during navigation transitions
- Navigation state (active item, collapsed/expanded) loads within 500ms on page refresh
- Performance is consistent across supported browsers (Chrome, Firefox, Edge, Safari)

**Priority:** High
**User Impact:** High

---

## 3. Functional Requirements

### REQ-NAV-101: Sidebar Structure & Layout

**Priority:** Critical
**User Impact:** High

#### Description

The left sidebar navigation is the primary mechanism for accessing PMM's six core modules. It must provide a persistent, visually clear, and accessible interface that supports both novice and expert users across all PMM pages.

#### User Flow

1. User opens PMM application or navigates to any PMM page
2. System renders left sidebar with:
   - Logo and branding at top
   - Six navigation items in middle section
   - Collapse/expand button at bottom
3. User identifies current module via orange active state highlight
4. User clicks on target module navigation item
5. System:
   - Updates active state to target module (orange highlight)
   - Removes active state from previous module
   - Navigates to target module's landing page
6. Sidebar remains visible and persistent on the new page

#### Business Rules

**Visual Hierarchy:**
- Logo area occupies top 80-100px of sidebar
- Navigation items are stacked vertically with consistent spacing (8-12px gap)
- Active item uses orange background (#F97316 or brand equivalent)
- Inactive items use transparent or dark blue background (#1E3A5F)
- All text is white for high contrast on dark background

**Navigation Order (Fixed):**
1. Dashboard (always first - primary landing page)
2. Inventario (core operational module)
3. Categorías (item organization)
4. Alertas (time-sensitive information)
5. Reportes (analysis and insights)
6. Configuración (always last - administrative)

This order cannot be customized by users to maintain consistency across installations.

**Sidebar Dimensions:**
- Expanded width: 240-256px
- Collapsed width: 64-72px
- Minimum height: 100vh (full viewport height)
- Sidebar is fixed position (does not scroll with content)

**Navigation Item Anatomy:**
Each navigation item consists of:
- Icon (24x24px) aligned to left with 16px left padding
- Text label with 12px left margin from icon
- 12px padding top/bottom for touch-friendly targets (minimum 44px total height)
- Rounded corners (4-6px border-radius) for active state background
- Hover state: slight brightness increase or subtle background color change

#### Data Requirements

**To Display:**
- Module icon (SVG or icon font)
- Module label (localized text)
- Active state (boolean)
- Module route/URL
- User permission to access module (for future role-based access)

**Immutable:**
- Navigation order (Dashboard → Inventario → Categorías → Alertas → Reportes → Configuración)
- Core module icons (maintain visual consistency)

#### Integration & Dependencies

**Frontend Routing:**
- Navigation integrates with Next.js App Router or equivalent routing system
- Each navigation item maps to a specific route:
  - Dashboard → `/` or `/dashboard`
  - Inventario → `/inventory`
  - Categorías → `/categories`
  - Alertas → `/alerts`
  - Reportes → `/reports`
  - Configuración → `/configuration`

**State Management:**
- Active module state is derived from current URL path
- Sidebar collapse/expand state is stored in:
  - LocalStorage (persists across sessions)
  - Session state (for current tab)
- Active state updates automatically on route changes

**Accessibility Integration:**
- Navigation is wrapped in `<nav>` semantic element with `aria-label="Main navigation"`
- Active item has `aria-current="page"` attribute
- Keyboard focus management coordinates with browser focus model

#### Edge Cases & Error Scenarios

**Narrow Viewport (< 768px):**
- Sidebar becomes overlay/drawer triggered by hamburger menu
- Default state is hidden (collapsed)
- Overlay includes close button and backdrop click-to-close

**JavaScript Disabled:**
- Navigation items remain as standard HTML links (`<a>` tags)
- Server-side navigation still functions
- Active state may rely on server-rendered class based on current path

**Slow Network:**
- Navigation remains interactive during page loads
- Active state changes immediately (optimistic UI)
- Loading indicator shows in main content area, not in navigation

**Module Permission Denied:**
- If user lacks permission for a module (future role-based access):
  - Item is visually disabled (reduced opacity)
  - Clicking shows "Access Denied" message
  - Item remains visible for discoverability but non-functional
  - Tooltip explains "Contact administrator for access"

**URL Manipulation:**
- If user manually navigates to a URL that doesn't match a navigation item (e.g., `/inventory/item/123`):
  - Parent module is highlighted (Inventario in this case)
  - Breadcrumb or page title provides additional context

**Logo Click from Dashboard:**
- Clicking logo while on Dashboard is a no-op (no page reload)
- Visual feedback (brief highlight) confirms click was registered

---

### REQ-NAV-102: Active State Management

**Priority:** Critical
**User Impact:** High

#### Description

The active state indicator (orange highlight) is the primary visual cue that tells users where they are in the application. Incorrect or missing active state leads to navigation errors and reduced confidence. Active state must be accurate, immediate, and visually distinct.

#### User Flow

1. User navigates to a module (via sidebar click, URL, or external link)
2. System determines current route/path
3. System identifies matching navigation item
4. System applies active state styling to matching item
5. System removes active state from any previously active item
6. User sees orange highlighted item and confirms location

#### Business Rules

**Single Active State:**
- Exactly one navigation item is active at any time
- If current path doesn't match a primary module (e.g., sub-page), the parent module remains active
- Example: `/inventory/categories/threads` → Inventario is active

**Active State Persistence:**
- Active state survives page refreshes
- Active state is consistent across browser tabs viewing the same module
- Active state cannot be manually toggled off (always at least one active)

**Visual Specification:**
- Background color: Orange (#F97316 or PMM brand orange)
- Text color: White (#FFFFFF)
- Icon color: White (#FFFFFF)
- Border-radius: 6px (rounded corners)
- Full-width background within sidebar (8px margin from sidebar edges)

**Inactive State Specification:**
- Background color: Transparent or subtle dark blue (#1E3A5F)
- Text color: White (#FFFFFF) or light gray (#E5E7EB)
- Icon color: White (#FFFFFF) or light gray (#E5E7EB)

**Hover State (Inactive Items):**
- Background color: Semi-transparent white (rgba(255, 255, 255, 0.1))
- Cursor: Pointer
- Transition: 150ms ease-in-out

**Hover State (Active Item):**
- No visual change (already highlighted)
- Cursor: Default or pointer (indicates no-op click)

#### Data Requirements

**To Determine Active State:**
- Current URL path or route
- Navigation item route mapping
- Parent-child route relationships (for sub-pages)

**To Display:**
- Active state boolean for each navigation item
- CSS classes or inline styles for active/inactive states

#### Integration & Dependencies

**Routing System:**
- Active state derivation integrates with Next.js `usePathname()` hook or equivalent
- Route matching logic handles exact matches and partial matches (for sub-routes)

**State Updates:**
- Active state recalculates on every route change
- No manual state management required (derived state pattern)

**Server-Side Rendering:**
- Active state is calculated on server for initial page load
- Prevents flash of incorrect active state on page load

#### Edge Cases & Error Scenarios

**404 or Error Pages:**
- If user lands on a 404 page within PMM:
  - No navigation item is active (all inactive)
  - OR Dashboard remains active as fallback
- Error pages maintain navigation structure for easy recovery

**Deep Links to Sub-Pages:**
- User clicks external link to `/reports/production-variance-report`
- Reportes navigation item shows active state
- Page breadcrumb or title shows full context

**Multi-Tab Behavior:**
- User opens Dashboard in Tab 1 (Dashboard active)
- User opens Inventario in Tab 2 (Inventario active)
- Each tab maintains independent active state
- Switching between tabs shows correct active state for that tab

**Rapid Navigation (Click Spam):**
- User rapidly clicks multiple navigation items
- Only the last valid click's active state is applied
- No race condition or multiple active items
- Navigation queue is handled by routing system (latest click wins)

**Browser Back/Forward:**
- User navigates: Dashboard → Inventario → Reportes
- User clicks browser Back button
- Active state updates to Inventario
- Active state follows browser history accurately

---

### REQ-NAV-103: Sidebar Collapse/Expand Functionality

**Priority:** High
**User Impact:** Medium

#### Description

The sidebar collapse feature allows users to maximize screen real estate for content-focused work (especially on smaller screens or multi-window setups) while maintaining quick access to navigation. This is essential for warehouse staff on tablets and power users with multiple applications open.

#### User Flow

**Collapse Flow:**
1. User clicks collapse button (chevron-left icon) at bottom of expanded sidebar
2. System animates sidebar width from 240px to 64px over 250ms
3. System hides text labels with fade-out animation
4. System centers icons in collapsed sidebar
5. System changes collapse button to expand button (chevron-right icon)
6. System saves collapsed state to localStorage
7. Main content area expands to fill reclaimed space

**Expand Flow:**
1. User clicks expand button (chevron-right icon) at bottom of collapsed sidebar
2. System animates sidebar width from 64px to 240px over 250ms
3. System shows text labels with fade-in animation (slight delay for smooth transition)
4. System changes expand button to collapse button (chevron-left icon)
5. System saves expanded state to localStorage
6. Main content area adjusts to accommodate sidebar

**Hover Tooltip Flow (Collapsed State):**
1. User hovers over a navigation icon in collapsed sidebar
2. System displays tooltip with module name after 500ms delay
3. Tooltip appears to the right of the sidebar (not overlapping content)
4. User moves mouse away
5. Tooltip disappears immediately

#### Business Rules

**State Persistence:**
- Collapse/expand state is saved to browser localStorage
- State persists across:
  - Page navigations within PMM
  - Browser sessions (closing and reopening)
  - Browser tabs (new tabs inherit last saved state)
- State is user-specific and device-specific (not synced across devices)

**Responsive Behavior:**
- On screens < 1024px width, sidebar defaults to collapsed on first visit
- On screens ≥ 1024px width, sidebar defaults to expanded on first visit
- User's manual preference overrides default behavior

**Animation Smoothness:**
- Width transition: 250ms cubic-bezier(0.4, 0.0, 0.2, 1)
- Text fade-out (collapse): 150ms, starts immediately
- Text fade-in (expand): 200ms, starts after 100ms delay
- No layout shift in main content area (smooth reflow)

**Collapsed State Appearance:**
- Sidebar width: 64px
- Icons: 24x24px, centered horizontally
- Logo: Shows compact version (icon only, no text)
- Active state: Vertical bar on left edge (4px wide, orange) instead of full background
- Collapse/expand button: Same position at bottom, icon changes

**Accessibility in Collapsed State:**
- Tooltips are ARIA-labeled for screen readers
- Keyboard focus still moves through navigation items
- Screen reader announces module name even without visible text
- Collapse/expand button label changes ("Expand navigation" / "Collapse navigation")

#### Data Requirements

**To Display:**
- Collapsed state (boolean)
- Tooltip text for each navigation item
- Icon-only version of logo

**To Store:**
- Collapsed state in localStorage: `pmm_sidebar_collapsed: "true" | "false"`
- Timestamp of last state change (for analytics)

#### Integration & Dependencies

**Layout System:**
- Main content area width is calculated as: `100vw - sidebarWidth`
- Responsive breakpoints adjust sidebar behavior:
  - `< 768px`: Sidebar becomes overlay (mobile)
  - `768px - 1024px`: Sidebar defaults to collapsed
  - `≥ 1024px`: Sidebar defaults to expanded

**CSS Framework:**
- Transitions use Tailwind CSS or equivalent
- Width changes are managed by CSS transitions (no JavaScript animation)
- Content reflow is handled by Flexbox or CSS Grid

**State Management:**
- Collapsed state is stored in React state (or equivalent)
- State is initialized from localStorage on component mount
- State changes are synced to localStorage immediately

#### Edge Cases & Error Scenarios

**localStorage Unavailable (Private Browsing):**
- Collapse/expand still functions
- State does not persist across page navigations
- User must re-collapse on each page load
- No error shown to user (graceful degradation)

**Narrow Screen + Expanded Sidebar:**
- If screen width < 900px and sidebar is expanded:
  - Sidebar may overlay content slightly
  - OR sidebar forces horizontal scroll
  - System shows subtle hint to collapse for better experience

**Rapid Toggle (Click Spam):**
- Animation completes before next toggle begins
- Multiple rapid clicks queue but don't break animation
- Final state matches final click

**Keyboard Navigation in Collapsed State:**
- Tab still moves through navigation items
- Enter activates focused item (works identically to expanded state)
- Tooltip does NOT appear on keyboard focus (only on hover)
- Screen reader announces full module name regardless of visual state

**Mid-Animation Navigation:**
- User clicks a navigation item during collapse/expand animation
- Navigation proceeds immediately
- Animation completes on destination page
- Collapsed state is preserved

**Print View:**
- When printing, sidebar collapses or hides entirely
- Main content expands to full page width
- Navigation is not included in printed output

---

### REQ-NAV-104: Top Bar - Notification System

**Priority:** Critical
**User Impact:** High

#### Description

The notification badge in the top bar provides real-time awareness of critical system alerts (low stock, production delays, quality issues) without requiring users to manually check the Alertas module. This ensures time-sensitive issues are addressed promptly, reducing operational risk.

#### User Flow

**Normal Flow:**
1. System detects a new alert condition (e.g., stock level drops below threshold)
2. System increments notification count
3. System updates notification badge in top bar to show new count
4. User sees red badge with count (e.g., "3") on bell icon
5. User clicks notification bell icon
6. System opens notification panel (dropdown) OR navigates to Alertas module
7. User views alert details
8. User acknowledges or takes action on alert
9. System decrements notification count
10. Badge updates or disappears if count reaches zero

**Real-Time Update Flow:**
1. User is viewing Dashboard
2. New critical alert is created (e.g., item goes out of stock)
3. Within 10 seconds, badge count increases from 3 to 4
4. User notices badge change (red color draws attention)
5. User investigates immediately

#### Business Rules

**Notification Count Logic:**
- Badge displays count of **unread/unacknowledged** alerts only
- Alerts are marked as "read" when:
  - User views the Alertas module
  - User clicks on specific alert in notification panel
  - User explicitly marks alert as acknowledged
- Badge shows actual count up to 9 (e.g., "1", "2", "9")
- Badge shows "9+" for counts ≥ 10 to maintain compact size

**Badge Visibility:**
- Badge is visible when count ≥ 1
- Badge is hidden when count = 0
- Badge appears on top-right of bell icon (overlapping)

**Alert Priority Levels (for future enhancement):**
- Critical (red): Production stopped, out of stock on key item
- High (orange): Low stock warning, cost variance > 20%
- Medium (yellow): Cycle count discrepancy, BOM approval pending
- Low (blue): General notifications, system updates

Badge color could change based on highest priority unread alert (future enhancement).

**Real-Time Synchronization:**
- Notification count updates via WebSocket or polling (every 30 seconds)
- Multiple browser tabs show synchronized count
- Count updates within 10 seconds of alert creation

**Visual Specification:**
- Badge background: Red (#EF4444)
- Badge text: White (#FFFFFF), bold, 11-12px font size
- Badge shape: Circle, minimum 18x18px (expands for larger counts like "9+")
- Badge position: Top-right corner of bell icon, overlapping by 50%
- Badge border: 2px white border for separation from background

#### Data Requirements

**To Display:**
- Total unread alert count (integer)
- Highest priority level of unread alerts (optional, for future color-coding)

**To Fetch:**
- Alert summary from backend API:
  ```json
  {
    "unreadCount": 3,
    "highestPriority": "critical",
    "alerts": [
      { "id": "ALT-001", "type": "out_of_stock", "priority": "critical", "timestamp": "2026-02-05T10:30:00Z" },
      { "id": "ALT-002", "type": "low_stock", "priority": "high", "timestamp": "2026-02-05T09:15:00Z" },
      { "id": "ALT-003", "type": "cost_variance", "priority": "medium", "timestamp": "2026-02-05T08:00:00Z" }
    ]
  }
  ```

#### Integration & Dependencies

**Backend API:**
- `GET /api/notifications/summary` returns unread count and recent alerts
- `POST /api/notifications/{id}/acknowledge` marks alert as read
- `GET /api/notifications` returns full alert list (for Alertas module)

**Real-Time Updates:**
- WebSocket connection subscribes to `user/{userId}/notifications` channel
- Server pushes notification events: `{ "type": "new_alert", "count": 4 }`
- Fallback: Polling every 30 seconds if WebSocket unavailable

**Alertas Module Integration:**
- Clicking bell icon navigates to `/alerts` OR opens dropdown panel
- Alertas module marks alerts as read when viewed
- Badge count decreases automatically when user views Alertas

**Permissions:**
- Users only see notifications for alerts they have permission to view
- Example: Warehouse staff see inventory alerts only, not cost variance alerts

#### Edge Cases & Error Scenarios

**WebSocket Disconnection:**
- System falls back to polling (every 30 seconds)
- User sees notification explaining possible delay: "Real-time updates temporarily unavailable"
- Badge still updates, just with potential 30-second lag

**Conflicting Counts Across Tabs:**
- User has PMM open in two browser tabs
- User acknowledges alert in Tab 1
- Tab 2's count decreases within 10 seconds (via WebSocket broadcast)
- If WebSocket fails, Tab 2 updates on next poll

**Very High Alert Count (> 99):**
- Badge shows "99+" to maintain size
- Clicking badge shows urgent message: "99+ unread alerts - please review immediately"
- System encourages bulk acknowledge or filter by priority

**Notification API Failure:**
- If API fails to load notification count:
  - Badge shows "!" (exclamation) instead of count
  - Tooltip says "Unable to load notifications - click to retry"
  - System retries in background
- User can still navigate to Alertas module manually

**User Clicks Badge During Load:**
- If user clicks bell icon before notification panel loads:
  - Show loading spinner in dropdown
  - Panel populates when data arrives
  - No error if data loads within 3 seconds

**Alert Acknowledged Elsewhere (Multi-Device):**
- User acknowledges alert on desktop
- User's tablet shows badge count decrease within 30 seconds
- Ensures consistency across devices for same user

**Zero Notifications State:**
- Badge is completely hidden (not "0")
- Clicking bell icon shows message: "No new alerts - all clear!"
- Positive reinforcement for good system state

---

### REQ-NAV-105: Top Bar - User Profile & Account Access

**Priority:** High
**User Impact:** Medium

#### Description

The user profile icon in the top bar provides quick access to account settings, help resources, and logout functionality. This centralizes user-level actions in a consistent location, reducing navigation overhead for account management tasks.

#### User Flow

**Standard Flow:**
1. User clicks user profile icon (avatar) in top-right of top bar
2. System displays dropdown menu with options:
   - View Profile
   - Account Settings
   - Help & Documentation
   - Log Out
3. User clicks desired option
4. System executes corresponding action

**Logout Flow:**
1. User clicks profile icon → Log Out
2. System shows confirmation modal: "Are you sure you want to log out?"
3. User confirms
4. System logs user out, clears session
5. System redirects to login page

**Profile View Flow:**
1. User clicks profile icon → View Profile
2. System navigates to `/profile` page
3. Page displays:
   - User name, email, role
   - Last login timestamp
   - Activity summary
   - Edit profile button

#### Business Rules

**Dropdown Menu Content:**
- **View Profile**: Links to `/profile` (user details page)
- **Account Settings**: Links to `/settings` (preferences, password change, notification settings)
- **Help & Documentation**: Links to `/help` or external documentation site
- **Log Out**: Triggers logout confirmation modal

**Dropdown Positioning:**
- Menu appears below and aligned to the right edge of profile icon
- Menu width: 200-220px
- Menu has drop shadow and slight border for depth
- Clicking outside menu closes it

**Profile Icon Display:**
- If user has profile photo: Display photo (circular, 36x36px)
- If no profile photo: Display initials (first letter of first and last name)
- Background color: Generated from user ID (consistent color per user)
- Initials: White text, 14px font size, uppercase

**Session Management:**
- Session timeout: 8 hours of inactivity
- If session expires, user sees "Session expired - please log in" message
- User is redirected to login page with return URL preserved

**Visual Specification:**
- Profile icon: 36x36px circle
- Hover state: Subtle border or glow effect
- Dropdown background: White (#FFFFFF)
- Dropdown items: 40px height, left-aligned text, hover background gray (#F3F4F6)
- Logout option: Red text (#EF4444) to indicate destructive action

#### Data Requirements

**To Display:**
- User full name
- User initials (if no photo)
- User profile photo URL (if available)
- User role (for display in profile page)

**To Fetch:**
- `GET /api/user/profile` returns:
  ```json
  {
    "id": "USR-001",
    "firstName": "Juan",
    "lastName": "Villaplana",
    "email": "juan@example.com",
    "role": "System Administrator",
    "profilePhotoUrl": null,
    "lastLogin": "2026-02-05T08:30:00Z"
  }
  ```

#### Integration & Dependencies

**Authentication System:**
- Profile icon only appears when user is authenticated
- Logout triggers authentication service's session termination
- Return URL is preserved on logout for seamless re-login

**Profile & Settings Pages:**
- View Profile navigates to dedicated profile page
- Account Settings navigates to settings page
- Both pages use same navigation structure (top bar, sidebar visible)

**Help Documentation:**
- Help link opens in new tab OR navigates to embedded help center
- Help content is context-aware (shows relevant section for current module)

#### Edge Cases & Error Scenarios

**Profile Photo Load Failure:**
- If photo URL returns 404 or fails to load:
  - Fall back to initials display
  - No error shown to user

**Logout Confirmation Declined:**
- User clicks Log Out → confirmation modal appears
- User clicks "Cancel"
- Modal closes, user remains logged in, dropdown menu closes

**Concurrent Session Logout:**
- User is logged out in another tab or device
- Current tab detects session invalidation within 30 seconds
- User sees modal: "You have been logged out. Please log in again."
- User is redirected to login page

**Dropdown Click-Outside Detection:**
- User opens profile dropdown
- User clicks anywhere outside dropdown (sidebar, content area)
- Dropdown closes immediately

**Keyboard Navigation:**
- Tab to profile icon, Enter to open dropdown
- Arrow keys navigate menu items
- Enter activates selected item
- Escape closes dropdown

**Rapid Clicks:**
- User rapidly clicks profile icon
- Dropdown toggles open/close
- No duplicate dropdowns or race conditions

---

### REQ-NAV-106: Application Title Bar

**Priority:** Medium
**User Impact:** Low

#### Description

The top bar displays "Pennant Mill Manager" application name for branding reinforcement and multi-tab identification. This helps users quickly identify the PMM tab among many open browser tabs.

#### User Flow

1. User opens PMM application
2. Top bar displays "Pennant Mill Manager" text in header area
3. User switches between browser tabs
4. User easily identifies PMM tab by application name in browser tab title and top bar

#### Business Rules

**Visual Specification:**
- Text: "Pennant Mill Manager"
- Font: Sans-serif, 18-20px, medium weight (500-600)
- Color: Dark gray (#1F2937) or navy to match brand
- Position: Left side of top bar (if logo not repeated) OR center
- Top bar height: 60-64px with centered text vertically

**Browser Tab Title:**
- Format: `[Current Module] - Pennant Mill Manager`
- Examples:
  - Dashboard: "Dashboard - Pennant Mill Manager"
  - Inventario: "Inventario - Pennant Mill Manager"
  - Specific item page: "Hilo de Algodón 20/2 - Inventario - Pennant Mill Manager"

**Responsive Behavior:**
- On mobile (< 768px), application name may be shortened to "PMM" or hidden
- Hamburger menu icon replaces application name on left side
- Full name visible in sidebar overlay when opened

#### Data Requirements

**To Display:**
- Application name (static text)
- Current module name (for browser tab title)
- Current page title (for detailed browser tab title)

#### Integration & Dependencies

**Branding Consistency:**
- Top bar application name matches sidebar logo text
- Color scheme aligns with PMM brand guidelines
- Font matches overall application typography

**SEO & Browser UX:**
- Browser tab title updates dynamically on navigation
- Favicon displays PMM logo icon (16x16px, 32x32px)

#### Edge Cases & Error Scenarios

**Very Narrow Screens:**
- If screen width < 640px:
  - Application name hidden or abbreviated to "PMM"
  - Space reserved for hamburger menu and icons

**Long Module Names:**
- Browser tab title truncates long module names:
  - "Production Variance Report - Reportes - Pennant..." (truncated by browser)

---

### REQ-NAV-107: Navigation Icons

**Priority:** High
**User Impact:** Medium

#### Description

Each navigation item uses a distinct icon to support visual scanning, improve accessibility, and aid users with language barriers or low literacy. Icons must be recognizable, semantically meaningful, and consistent with industry standards.

#### User Flow

1. User scans sidebar for target module
2. User recognizes module by icon shape/symbol (especially in collapsed mode)
3. User clicks icon or text to navigate

#### Business Rules

**Icon Specifications:**

| Module | Icon | Description |
|--------|------|-------------|
| **Dashboard** | Grid of 4 squares (dashboard/widgets icon) | Represents overview/summary |
| **Inventario** | 3D box/cube | Universal symbol for inventory/stock |
| **Categorías** | Folder or tag icon | Represents organization/categorization |
| **Alertas** | Triangle with exclamation mark | Warning/alert symbol |
| **Reportes** | Bar chart or document with graph | Represents analytics/reports |
| **Configuración** | Gear/cog icon | Universal settings symbol |

**Technical Specifications:**
- Icon format: SVG (scalable, crisp at any size)
- Icon size: 24x24px (both expanded and collapsed modes)
- Icon color: White (#FFFFFF) in both active and inactive states
- Icon stroke width: 1.5-2px for consistent visual weight
- Icons use outline style (not filled) for modern, clean look

**Icon Library:**
- Use consistent icon library across application (e.g., Heroicons, Lucide, Phosphor)
- Custom icons match library style if needed

**Accessibility:**
- Icons have `aria-hidden="true"` (decorative, text label is primary)
- In collapsed mode, icons have `aria-label` with module name
- Screen readers announce text label, not icon description

#### Data Requirements

**To Display:**
- Icon component or SVG path for each module
- Icon color (dynamic based on active/inactive state)

#### Integration & Dependencies

**Icon Library:**
- Icons imported from chosen icon library
- Icon components are tree-shakeable (only used icons bundled)

**Theme System:**
- Icon color adapts if dark mode is implemented
- Icons remain visible on all background colors (white on dark ensures high contrast)

#### Edge Cases & Error Scenarios

**Icon Load Failure:**
- If SVG fails to load:
  - Text label remains visible (navigation still usable)
  - Placeholder icon or first letter of module name shown

**Custom Icons:**
- If custom icon is added for new module:
  - Must match size, stroke width, and style of existing icons
  - Accessibility attributes must be consistent

---

### REQ-NAV-108: Responsive Navigation (Mobile/Tablet)

**Priority:** High
**User Impact:** High

#### Description

On mobile and tablet devices, the sidebar transitions to an overlay/drawer navigation to maximize screen space for content. This is critical for warehouse staff using tablets and managers checking data on phones.

#### User Flow

**Mobile Navigation Open Flow:**
1. User opens PMM on mobile device (screen width < 768px)
2. Sidebar is hidden by default
3. Hamburger menu icon (☰) is visible in top-left of top bar
4. User taps hamburger icon
5. Sidebar slides in from left as overlay (covers content)
6. Overlay backdrop (semi-transparent black) appears behind sidebar
7. User taps navigation item
8. System navigates to module
9. Sidebar automatically closes
10. User sees content in full screen

**Mobile Navigation Close Flow:**
- User taps close button (X) in top-right of sidebar overlay
- OR User taps backdrop (outside sidebar)
- OR User taps a navigation item (auto-close after navigation)
- Sidebar slides out to left
- Backdrop fades out

#### Business Rules

**Responsive Breakpoints:**
- **< 768px (Mobile)**: Sidebar becomes overlay, hamburger menu shown
- **768px - 1024px (Tablet)**: Sidebar visible but defaults to collapsed
- **≥ 1024px (Desktop)**: Sidebar visible and defaults to expanded

**Mobile Overlay Specifications:**
- Sidebar width: 280px (wider than desktop for touch targets)
- Animation: Slide-in from left, 300ms ease-out
- Backdrop: rgba(0, 0, 0, 0.5), fades in with sidebar
- Z-index: Sidebar 100, Backdrop 90, Main content 1
- Body scroll: Disabled while overlay is open (prevent background scrolling)

**Touch Targets:**
- Minimum touch target size: 44x44px (WCAG accessibility)
- Navigation items: 48px height on mobile (larger than desktop's 44px)
- Hamburger icon: 44x44px tap area

**Gestures:**
- Swipe right from left edge: Opens sidebar
- Swipe left on sidebar: Closes sidebar
- Tap outside sidebar: Closes sidebar

**Top Bar on Mobile:**
- Height: 56px (standard mobile header height)
- Hamburger menu: Left side (replaces logo)
- Application name: Center (optional, may be hidden for space)
- Notification icon: Right side (maintained)
- User profile icon: Right side (maintained)

#### Data Requirements

**To Display:**
- Screen width (to determine responsive mode)
- Sidebar open/closed state (for mobile overlay)

**To Store:**
- Mobile sidebar state NOT persisted (always closed on page load)

#### Integration & Dependencies

**Responsive Layout:**
- CSS media queries trigger layout changes at breakpoints
- JavaScript detects screen width for gesture handling

**Touch Event Handling:**
- Swipe gestures use touch events (touchstart, touchmove, touchend)
- Tap outside uses click event on backdrop element

**Body Scroll Lock:**
- When sidebar overlay opens, body scroll is disabled
- When sidebar closes, body scroll is re-enabled
- Prevents awkward dual-scrolling behavior

#### Edge Cases & Error Scenarios

**Orientation Change (Tablet):**
- User rotates tablet from portrait to landscape
- If width crosses 1024px threshold, sidebar transitions from overlay to persistent
- Transition is smooth, no layout jump

**Viewport Resize (Mobile to Desktop):**
- User is on mobile, sidebar is overlay
- User rotates to landscape OR opens on larger screen
- Sidebar transitions to persistent (expanded or collapsed based on saved state)

**Sidebar Open During Navigation:**
- User opens sidebar overlay on mobile
- User taps navigation item
- Page navigation begins
- Sidebar closes immediately (doesn't wait for page load)
- New page loads with sidebar closed

**Swipe Gesture Conflicts:**
- User swipes right on a carousel or image slider
- System distinguishes:
  - Swipe from left edge (< 20px from edge): Opens sidebar
  - Swipe from content area: Interacts with content, not sidebar

**Backdrop Click During Animation:**
- User opens sidebar, immediately clicks backdrop before animation completes
- Sidebar reverses and closes
- No visual glitch or stuck state

**Keyboard Navigation on Mobile:**
- User navigates with external keyboard on tablet
- Tab key moves through navigation items
- Enter key activates navigation item
- Escape key closes sidebar overlay

---

### REQ-NAV-109: Keyboard Shortcuts

**Priority:** Medium
**User Impact:** Medium

#### Description

Keyboard shortcuts enable power users to navigate between modules instantly without mouse interaction, significantly improving workflow efficiency for users who frequently switch contexts (e.g., cost accountants reviewing multiple reports).

#### User Flow

1. User presses keyboard shortcut (e.g., `Alt+I` for Inventario)
2. System detects key combination
3. System navigates to target module
4. Active state updates immediately
5. User's keyboard focus moves to main content area (ready for interaction)

#### Business Rules

**Shortcut Definitions:**

| Module | Shortcut (Windows/Linux) | Shortcut (Mac) | Alternative |
|--------|--------------------------|----------------|-------------|
| Dashboard | `Alt+D` | `Cmd+D` | `Ctrl+1` |
| Inventario | `Alt+I` | `Cmd+I` | `Ctrl+2` |
| Categorías | `Alt+C` | `Cmd+C` | `Ctrl+3` |
| Alertas | `Alt+A` | `Cmd+A` | `Ctrl+4` |
| Reportes | `Alt+R` | `Cmd+R` | `Ctrl+5` |
| Configuración | `Alt+S` | `Cmd+S` | `Ctrl+6` |

**Shortcut Discovery:**
- Shortcuts are documented in Help section
- Tooltips on navigation items show shortcut (e.g., "Dashboard (Alt+D)")
- `Alt+/` or `Cmd+/` opens shortcut cheat sheet modal

**Conflict Avoidance:**
- Shortcuts must not conflict with browser defaults:
  - `Ctrl+T` (new tab), `Ctrl+W` (close tab), `Ctrl+R` (refresh) are avoided
  - `Alt` modifier reduces conflicts on Windows/Linux
  - `Cmd` modifier is standard on Mac
- If conflict exists, alternative shortcut is used

**Shortcut Behavior:**
- Shortcuts work globally (from any page within PMM)
- Shortcuts do NOT trigger when focus is in text input fields (prevents accidental navigation while typing)
- Shortcuts show brief visual feedback (flash active item before navigating)

**Accessibility:**
- Shortcuts are announced by screen readers ("Navigating to Inventario")
- Shortcuts follow WCAG accessibility guidelines
- Users can disable shortcuts in accessibility settings

#### Data Requirements

**To Display:**
- Shortcut key combination for each module
- Platform detection (Windows/Linux/Mac) for correct modifier key

**To Store:**
- User preference: Shortcuts enabled/disabled (default: enabled)

#### Integration & Dependencies

**Keyboard Event Handling:**
- Global keyboard listener captures shortcut combinations
- Listener checks if target is text input field (if yes, ignore shortcut)
- Listener triggers navigation via routing system

**Platform Detection:**
- Detect OS from user agent or `navigator.platform`
- Display correct modifier key in tooltips and documentation

#### Edge Cases & Error Scenarios

**Shortcut Pressed in Text Field:**
- User is typing in search box, presses `Alt+I`
- Shortcut is ignored
- Text input continues normally

**Shortcut During Page Load:**
- User presses `Alt+R` while Dashboard is loading
- Navigation to Reportes is queued
- When Dashboard load completes, navigation to Reportes proceeds immediately

**Conflicting Browser Extension:**
- User has browser extension that uses `Alt+D`
- PMM shortcut may not trigger (browser extension wins)
- User can use alternative shortcut `Ctrl+1`

**Rapid Shortcut Presses:**
- User presses `Alt+I`, then immediately `Alt+R`
- Only the last shortcut (`Alt+R`) is executed
- No double navigation or race condition

**Shortcut on Current Module:**
- User is on Dashboard, presses `Alt+D`
- Navigation is a no-op (no page reload)
- Visual feedback (brief highlight) confirms shortcut registered

---

### REQ-NAV-110: Navigation Performance & Loading States

**Priority:** High
**User Impact:** High

#### Description

Navigation interactions must feel instant and responsive, even on slower networks or devices. Clear loading states prevent user confusion and reduce perceived wait time.

#### User Flow

**Fast Navigation (< 1 second):**
1. User clicks navigation item
2. Active state changes immediately (< 100ms)
3. Page content loads and displays
4. Total time < 1 second
5. No loading indicator shown (feels instant)

**Slow Navigation (1-3 seconds):**
1. User clicks navigation item
2. Active state changes immediately
3. Loading spinner appears in main content area after 500ms
4. Page content loads and displays
5. Spinner disappears
6. Total time 1-3 seconds

**Very Slow Navigation (> 3 seconds):**
1. User clicks navigation item
2. Active state changes immediately
3. Loading spinner appears after 500ms
4. Progress message appears after 3 seconds ("Loading large report...")
5. Page content loads and displays
6. Total time > 3 seconds

#### Business Rules

**Performance Targets:**
- **Interaction feedback**: < 100ms (active state change, button press feedback)
- **Page navigation**: < 1 second (80th percentile on typical network)
- **Module load (no data)**: < 500ms (empty Dashboard, first-time Config page)
- **Module load (with data)**: < 2 seconds (Dashboard with KPIs, Inventario list)
- **Large reports**: < 5 seconds (Production Variance Report with 1000 rows)

**Loading State Progression:**
1. **0-500ms**: No loading indicator (optimistic UI, feels instant)
2. **500ms-3s**: Spinner in content area
3. **3s+**: Spinner + progress message ("Loading...")
4. **10s+**: Spinner + detailed message + cancel option

**Visual Loading Indicators:**
- Spinner: Centered in main content area, 48x48px, orange color
- Skeleton screens: For list views (Inventario, Reportes), show placeholder cards while loading
- Progress bar: For known-duration operations (large report generation)

**Navigation Remains Interactive:**
- User can click another navigation item while current page is loading
- Previous navigation is canceled, new navigation begins
- No disabled state on navigation items during load

**Error Handling:**
- If page fails to load within 10 seconds:
  - Show error message: "Unable to load [Module]. Please try again."
  - Provide "Retry" button
  - Provide "Go to Dashboard" fallback option
- Active state reverts to previous module if navigation fails

#### Data Requirements

**To Monitor:**
- Navigation start timestamp
- Page load completion timestamp
- Total navigation time (for analytics)
- Failed navigation count (for reliability metrics)

**To Display:**
- Loading state (boolean)
- Loading message (string, for slow loads)

#### Integration & Dependencies

**Frontend Routing:**
- Next.js App Router provides built-in loading states
- Loading UI is shown automatically for slow page loads

**Performance Monitoring:**
- Navigation times logged to analytics
- Slow navigation alerts sent to development team
- Performance regression detection

**Caching:**
- Frequently accessed modules (Dashboard, Inventario) are prefetched
- Static assets (icons, CSS) are cached aggressively
- API responses are cached where appropriate (stale-while-revalidate)

#### Edge Cases & Error Scenarios

**Network Offline:**
- User clicks navigation item while offline
- Browser shows offline error
- Navigation UI provides clear message: "Unable to load - check your connection"
- User can retry when back online

**API Timeout:**
- Dashboard API takes > 10 seconds to respond
- Page shows timeout error
- User can retry or navigate to another module

**Partial Page Load:**
- Dashboard loads, but KPI API fails
- Page shows successfully with error state for failed KPI cards
- User can still navigate, page is not blocked

**Cancel Navigation:**
- User clicks Inventario (slow load begins)
- User clicks Dashboard before Inventario loads
- Inventario load is canceled
- Dashboard load begins immediately
- Only one navigation occurs

---

## 4. Non-Functional Requirements

### NFR-NAV-201: Performance

**Requirement:** All navigation interactions must feel instant and responsive

**Specific Targets:**
- Active state change: < 100ms from click
- Sidebar collapse/expand animation: 250-300ms total duration
- Navigation to new module: < 3 seconds (80th percentile)
- Notification badge update: < 10 seconds from server event
- Keyboard shortcut response: < 100ms

**Measurement:**
- Frontend performance monitoring (e.g., Vercel Analytics, Sentry)
- Real User Monitoring (RUM) data collection
- 80th percentile response times tracked weekly

**Business Impact:**
- Slow navigation reduces productivity (15-30 module switches per user per day)
- Delays > 1 second feel sluggish and reduce user confidence
- Manufacturing environments require quick decision-making

---

### NFR-NAV-202: Accessibility (WCAG 2.1 AA Compliance)

**Requirement:** Navigation system must be fully accessible to users with disabilities

**Specific Targets:**
- **Keyboard Navigation**: 100% of navigation functions accessible via keyboard
- **Screen Reader**: All navigation elements properly labeled with ARIA attributes
- **Color Contrast**: Minimum 4.5:1 contrast ratio for text, 3:1 for icons
- **Touch Targets**: Minimum 44x44px on mobile devices
- **Focus Indicators**: Clear visual focus state for keyboard users

**Compliance Checklist:**
- Navigation wrapped in `<nav>` semantic element
- Active item has `aria-current="page"`
- Collapse/expand button has descriptive label
- Notification badge announces count to screen readers
- Skip navigation link provided
- Keyboard shortcuts documented and configurable

**Testing:**
- Automated accessibility scans (axe, WAVE)
- Manual keyboard-only navigation testing
- Screen reader testing (NVDA, JAWS, VoiceOver)
- User testing with accessibility advocates

**Business Impact:**
- Legal compliance (ADA, Section 508 for government contracts)
- Inclusive design expands user base
- Reduced support burden from accessibility-related issues

---

### NFR-NAV-203: Browser Compatibility

**Requirement:** Navigation system must work consistently across all supported browsers

**Supported Browsers:**
- Chrome 100+ (Windows, Mac, Linux)
- Firefox 100+ (Windows, Mac, Linux)
- Safari 15+ (Mac, iOS)
- Edge 100+ (Windows)

**Testing Strategy:**
- Automated cross-browser testing (BrowserStack, Playwright)
- Manual testing on primary browsers for each release
- Progressive enhancement for older browsers (graceful degradation)

**Known Limitations:**
- CSS animations may be reduced on older browsers (no collapse animation)
- WebSocket real-time updates fall back to polling on unsupported browsers

---

### NFR-NAV-204: Mobile Performance

**Requirement:** Navigation must be responsive and performant on mobile devices

**Specific Targets:**
- Sidebar overlay open/close: < 300ms animation
- Touch gesture response: < 100ms
- Hamburger menu tap: Immediate visual feedback
- Mobile page load: < 3 seconds on 3G connection

**Mobile-Specific Considerations:**
- Reduced animations on low-end devices (detected via `navigator.hardwareConcurrency`)
- Touch targets minimum 44x44px
- No hover-dependent interactions (hover tooltips replaced with tap behavior)

**Testing:**
- Real device testing (iOS, Android tablets and phones)
- Chrome DevTools mobile emulation
- Network throttling to simulate 3G/4G conditions

---

### NFR-NAV-205: Scalability (Future Modules)

**Requirement:** Navigation structure must support additional modules without redesign

**Scalability Targets:**
- Support up to 12 primary navigation items (currently 6)
- Support sub-navigation (nested menus) for complex modules
- Support role-based visibility (show/hide modules per user role)

**Design Considerations:**
- Sidebar height scrolls if > 12 items (currently fixed)
- Grouping/categorization for > 8 items (e.g., "Operations", "Administration")
- Horizontal or mega-menu navigation for very complex systems (future consideration)

**Future-Proofing:**
- Navigation item data is configuration-driven (not hardcoded)
- Icon library supports custom icons for new modules
- Color theming supports brand variations

---

### NFR-NAV-206: Data Security & Privacy

**Requirement:** Navigation system must protect user data and respect privacy

**Specific Targets:**
- No sensitive data in localStorage (only UI preferences like collapsed state)
- User profile data transmitted over HTTPS only
- Session tokens not exposed in browser console or network logs
- Notification badge does not reveal sensitive alert details (only count)

**Compliance:**
- GDPR: User can clear localStorage preferences
- SOC 2: Audit trail for logout events
- HIPAA (if applicable): No PHI in navigation components

---

### NFR-NAV-207: Internationalization (i18n)

**Requirement:** Navigation labels must support multiple languages

**Supported Languages (Initial):**
- Spanish (primary, as shown in screenshot: "Inventario", "Categorías")
- English (secondary)

**Implementation:**
- Navigation labels use translation keys (e.g., `nav.dashboard`, `nav.inventory`)
- Icons remain consistent across languages (universal symbols)
- RTL language support (future): Sidebar flips to right side for Arabic, Hebrew

**User Language Selection:**
- Language preference stored in user account settings
- Browser language detected on first visit
- Language switcher in user profile dropdown (future enhancement)

---

## 5. Content & UX Copy Requirements

### 5.1 Navigation Labels

**Business Terminology:**

PMM navigation uses clear, business-aligned terminology that reflects manufacturing operations:

| Module | Label (Spanish) | Label (English) | Rationale |
|--------|-----------------|-----------------|-----------|
| Dashboard | Dashboard | Dashboard | Universal term, recognized internationally |
| Inventario | Inventario | Inventory | Core manufacturing term |
| Categorías | Categorías | Categories | Item organization context |
| Alertas | Alertas | Alerts | Urgency and attention |
| Reportes | Reportes | Reports | Analysis and insights |
| Configuración | Configuración | Configuration | System administration |

**Avoid Generic Terms:**
- ❌ "Home" → ✅ "Dashboard" (more specific, business-oriented)
- ❌ "Items" → ✅ "Inventario" (broader than just items, includes stock)
- ❌ "Settings" → ✅ "Configuración" (aligned with Spanish primary language)

---

### 5.2 Tooltips

**Navigation Item Tooltips (Collapsed Sidebar):**
- Dashboard: "Dashboard - Resumen general" (Alt+D)
- Inventario: "Inventario - Gestión de stock" (Alt+I)
- Categorías: "Categorías - Organización de items" (Alt+C)
- Alertas: "Alertas - Notificaciones críticas" (Alt+A)
- Reportes: "Reportes - Análisis y datos" (Alt+R)
- Configuración: "Configuración - Ajustes del sistema" (Alt+S)

**Other Tooltips:**
- Collapse button: "Colapsar navegación"
- Expand button: "Expandir navegación"
- Notification icon: "3 alertas no leídas - Haga clic para ver"
- User profile icon: "Perfil de usuario - [User Name]"

**Tooltip Behavior:**
- Appear after 500ms hover delay
- Positioned to right of sidebar (collapsed mode) or below icon (top bar)
- Dismiss on mouse move away or click

---

### 5.3 Notification Messages

**Notification Badge:**
- Zero state: (badge hidden)
- 1-9 notifications: Show count "1", "2", "9"
- 10+ notifications: Show "9+"

**Notification Panel/Dropdown (Future):**
- Empty state: "No hay alertas nuevas - ¡Todo en orden!"
- Error state: "No se pueden cargar las notificaciones - Haga clic para reintentar"
- High count: "99+ alertas no leídas - Revise con urgencia"

---

### 5.4 Loading & Error Messages

**Loading States:**
- Fast load (< 500ms): (no message)
- Standard load (500ms-3s): (spinner only)
- Slow load (3s-10s): "Cargando [Module]..."
- Very slow load (> 10s): "Esto está tardando más de lo esperado. Espere un momento..."

**Error Messages:**
- Navigation failure: "No se pudo cargar [Module]. Por favor, inténtelo de nuevo."
- Network error: "Sin conexión - Verifique su conexión a Internet"
- Session timeout: "Su sesión ha expirado. Por favor, inicie sesión nuevamente."
- Permission denied: "No tiene permisos para acceder a [Module]. Contacte al administrador."

**Error Actions:**
- Primary: "Reintentar"
- Secondary: "Ir al Dashboard" (safe fallback)
- Tertiary: "Reportar problema" (link to support)

---

### 5.5 Logout Confirmation

**Confirmation Modal:**
- **Title:** "¿Cerrar sesión?"
- **Message:** "¿Está seguro de que desea cerrar sesión? Los cambios no guardados se perderán."
- **Primary Action:** "Cerrar sesión" (red, destructive)
- **Secondary Action:** "Cancelar" (gray, safe)

**Post-Logout:**
- Redirect to login page with message: "Sesión cerrada exitosamente"

---

### 5.6 Keyboard Shortcut Help

**Shortcut Cheat Sheet Modal (triggered by `Alt+/`):**

```
Atajos de Teclado - Pennant Mill Manager

Navegación:
  Alt+D  →  Dashboard
  Alt+I  →  Inventario
  Alt+C  →  Categorías
  Alt+A  →  Alertas
  Alt+R  →  Reportes
  Alt+S  →  Configuración

Acciones:
  Alt+/  →  Mostrar/ocultar este menú
  Esc    →  Cerrar paneles abiertos

Navegación con teclado:
  Tab       →  Siguiente elemento
  Shift+Tab →  Elemento anterior
  Enter     →  Activar elemento seleccionado
```

---

## 6. Dependencies & Constraints

### 6.1 Technical Dependencies

**Frontend Framework:**
- Next.js 16 (App Router required for SSR navigation state)
- React 19 for component architecture
- Tailwind CSS 4 for styling and responsive breakpoints

**Icon Library:**
- Heroicons, Lucide, or Phosphor Icons (must choose one for consistency)
- SVG format for scalability

**State Management:**
- React Context or Zustand for sidebar collapse state
- LocalStorage for persistence

**Real-Time Updates:**
- WebSocket for notification badge updates (Socket.IO or native WebSocket)
- Fallback to polling (30-second interval) if WebSocket unavailable

**Authentication:**
- Session management system (JWT or session cookies)
- Logout API endpoint
- Session timeout detection

---

### 6.2 PMM Module Dependencies

**Navigation Integrates With:**
- **Dashboard Module**: Default landing page, activated by logo click
- **Inventario Module**: Accessed via navigation, provides inventory data
- **Categorías Module**: Item categorization system
- **Alertas Module**: Notification source for badge count
- **Reportes Module**: Report listing and generation
- **Configuración Module**: System settings and user preferences

**Data Flow:**
- Alertas Module → Notification API → Top Bar Badge
- User Profile API → User Profile Icon (initials, photo)
- Current Route → Active State Calculation

---

### 6.3 Business Constraints

**User Permissions (Future Role-Based Access):**
- Not all users have access to all modules
- Example: Warehouse staff may not access Configuración
- Navigation items for restricted modules are disabled or hidden

**Multi-Site Operations:**
- Navigation is site-independent (same structure across all sites)
- Site selection happens within modules, not in navigation

**Historical Data Integrity:**
- Navigation actions are logged for audit trail
- Logout events are logged with timestamp and user ID

---

### 6.4 Design Constraints

**Brand Guidelines:**
- Orange primary color (#F97316) for active states and branding
- Dark blue background (#1E3A5F) for sidebar
- White text for high contrast
- Consistent with PMM logo and overall application design

**Accessibility Requirements:**
- WCAG 2.1 AA compliance (4.5:1 text contrast, 3:1 icon contrast)
- Keyboard navigation required
- Screen reader support required

**Mobile Constraints:**
- Sidebar overlay must not block critical top bar functions
- Touch targets minimum 44x44px
- Swipe gestures must not conflict with content interactions

---

### 6.5 Performance Constraints

**Network Conditions:**
- System must be usable on 3G connections (warehouse environments)
- Notification updates may have 10-30 second delay on slow networks
- Critical navigation functions (sidebar click) must work offline (local state change)

**Device Constraints:**
- Must support older tablets (3-4 years old) in warehouse environments
- Reduced animations on low-end devices

---

### 6.6 Internationalization Constraints

**Primary Language:**
- Spanish is primary (as shown in screenshot)
- English is secondary
- Future languages: Portuguese (Brazil), French (future expansion markets)

**RTL Language Support:**
- Not required for initial release
- Architecture must support future RTL implementation

---

## 7. Rollout & Change Management Strategy

### 7.1 Rollout Phases

**Phase 1: Internal Testing (Week 1-2)**
- Deploy to staging environment
- Internal user acceptance testing (UAT) with 5-10 power users
- Focus on keyboard navigation, accessibility, and performance
- Collect feedback on navigation flow and usability

**Phase 2: Pilot Rollout (Week 3-4)**
- Deploy to 20% of production users (selected sites)
- Monitor navigation analytics (time to navigate, error rates)
- Collect user feedback via in-app survey
- Iterate on any critical issues

**Phase 3: Full Rollout (Week 5-6)**
- Deploy to 100% of production users
- Monitor performance and support tickets
- Provide training materials (video tutorials, help documentation)
- Announce new navigation features via in-app notification

**Rollback Plan:**
- If critical issues arise, revert to previous navigation system
- Maintain previous navigation as fallback for 2 weeks post-rollout

---

### 7.2 User Training & Communication

**Training Materials:**
- **Video Tutorial (2 minutes)**: Overview of new navigation, keyboard shortcuts, mobile overlay
- **Help Documentation**: Keyboard shortcuts, navigation tips, FAQs
- **In-App Onboarding**: First-time users see brief overlay highlighting navigation features

**Communication Plan:**
- **Pre-Launch (1 week before)**: Email announcement with video tutorial
- **Launch Day**: In-app notification with "What's New" highlights
- **Post-Launch (1 week after)**: Follow-up email with keyboard shortcut cheat sheet

**Support Preparation:**
- Support team trained on new navigation system
- FAQ document created for common questions
- Support ticket category created for navigation issues

---

### 7.3 Success Criteria for Rollout

**Go/No-Go Decision Criteria (End of Pilot Phase):**
- Navigation error rate < 5% (clicks on wrong module)
- User satisfaction rating ≥ 4.0/5.0
- Navigation-related support tickets < 10 per week (for 100-user pilot)
- No critical accessibility issues reported
- Average navigation time < 3 seconds (80th percentile)

**Post-Rollout Monitoring (First 30 Days):**
- Daily monitoring of navigation analytics
- Weekly review of support tickets
- Monthly user satisfaction survey
- Performance regression alerts

---

### 7.4 Backward Compatibility

**Current System:**
- If existing PMM installation has different navigation structure, migration is seamless
- User preferences (collapsed state) are reset on first load of new navigation
- No data migration required (navigation is UI-only)

**Future Updates:**
- Navigation structure is versioned to support future enhancements
- New modules can be added without breaking existing navigation
- Keyboard shortcuts are configurable to avoid conflicts with future features

---

### 7.5 Accessibility Rollout

**Accessibility Champion Program:**
- Recruit 2-3 users with accessibility needs to test navigation
- Provide early access (1 week before pilot)
- Collect detailed feedback on keyboard navigation, screen reader compatibility
- Iterate on accessibility issues before full rollout

**Accessibility Certification:**
- External accessibility audit (WCAG 2.1 AA)
- Fix all critical issues before full rollout
- Document accessibility features in help center

---

## 8. Handoff Notes

### 8.1 For Architecture & Development Teams

This PRD defines **business requirements and user experience expectations** for the PMM Navigation System. The following technical decisions are left to the architecture and development teams:

**Frontend Architecture:**
- Choice of state management library (React Context, Zustand, Redux)
- Component structure and file organization
- CSS implementation strategy (Tailwind utilities vs. CSS modules)
- Animation library (CSS transitions, Framer Motion, React Spring)

**Backend API Design:**
- Notification API endpoints and data schema
- User profile API structure
- WebSocket implementation (Socket.IO, native WebSocket, SSE)
- Session management and logout flow

**Performance Optimization:**
- Caching strategy for navigation state
- Prefetching strategy for modules
- Code splitting and lazy loading
- Image optimization for icons and logos

**Testing Strategy:**
- Unit testing approach for navigation components
- Integration testing for navigation flows
- Accessibility testing tools and processes
- Performance testing and benchmarking

### 8.2 Open Questions for Architecture Review

1. **Real-Time Notifications**: WebSocket vs. Server-Sent Events vs. Polling for notification badge updates?
2. **Mobile Gesture Library**: Native touch events vs. third-party library (e.g., React Swipeable)?
3. **Navigation State Persistence**: LocalStorage vs. Server-side user preferences?
4. **Icon Library**: Heroicons vs. Lucide vs. Phosphor Icons (team preference)?
5. **Keyboard Shortcut Conflicts**: How to handle browser extension conflicts programmatically?

### 8.3 Success Metrics Tracking

The Product Manager will track the following metrics post-launch:

- **Navigation Efficiency**: Average time to navigate (< 3 seconds target)
- **Adoption of Advanced Features**: % users accessing Reportes and Configuración weekly
- **Error Reduction**: Navigation error rate (< 2% target)
- **Support Impact**: Navigation-related support tickets (baseline vs. 3 months post-launch)
- **Accessibility Compliance**: WCAG audit score (AA compliance target)
- **User Satisfaction**: Quarterly survey rating (≥ 4.2/5.0 target)

### 8.4 Future Enhancements (Out of Scope for v1.0)

The following features are identified as valuable but deferred to future releases:

**Role-Based Navigation (v2.0):**
- Show/hide modules based on user permissions
- Custom navigation for different user roles (e.g., warehouse view vs. admin view)

**Notification Panel (v1.1):**
- Dropdown panel showing recent alerts (not just count)
- Quick actions on notifications (acknowledge, dismiss, view details)
- Notification filtering and search

**Favorites/Pinned Pages (v2.0):**
- Users can pin frequently accessed sub-pages to navigation
- Customizable quick links section

**Navigation Search (v2.0):**
- Global search within navigation (find module or page by name)
- Keyboard shortcut: `Ctrl+K` or `Cmd+K`

**Dark Mode (v3.0):**
- Dark theme for navigation and entire application
- User preference stored in settings

**Sub-Navigation/Mega Menus (v2.0):**
- Expandable sub-menus for complex modules (e.g., Reportes → Production Reports, Inventory Reports)
- Hover or click to expand sub-navigation

**Navigation Analytics Dashboard (Internal):**
- Heatmap of most-used navigation items
- Average time to navigate per user cohort
- Identification of unused modules for UX improvement

---

## Appendix: Visual Design Reference

### Screenshot Analysis

Based on the provided screenshot (`Screenshot 2026-02-05 005814.png`), the following visual specifications are observed:

**Sidebar:**
- Background color: Dark blue (#1E3A5F or similar)
- Width: ~240px (expanded)
- Logo area: Orange icon (#F97316) with "Pennant Mill Manager" white text
- Active item (Dashboard): Orange background (#F97316), white text, full-width rounded rectangle
- Inactive items: White text, no background, subtle hover effect
- Icons: 24x24px, white color, outline style
- Collapse button: Chevron-left icon, bottom of sidebar, white color

**Top Bar:**
- Background color: White (#FFFFFF)
- Height: ~60px
- Application title: "Pennant Mill Manager" in dark gray (#1F2937)
- Notification icon: Bell icon with red badge showing "3"
- User profile icon: Circular avatar, 36x36px

**Main Content Area:**
- Background color: Light gray (#F3F4F6)
- Page title: "Dashboard" in large black text
- Subtitle: "Resumen general del inventario" in gray text
- KPI cards: White background, rounded corners, shadow

**Color Palette:**
- Primary orange: #F97316
- Dark blue: #1E3A5F
- White: #FFFFFF
- Light gray: #F3F4F6
- Dark gray/black: #1F2937
- Red (notifications): #EF4444

**Typography:**
- Sans-serif font family (likely Inter, Roboto, or similar)
- Navigation items: 14-16px font size
- Page titles: 24-28px font size
- Subtitles: 14-16px font size, lighter weight

---

## Document Approval

This PRD is submitted for review and approval by:

- **Product Manager**: [Name]
- **Engineering Lead**: [Name]
- **UX/UI Designer**: [Name]
- **Stakeholders**: Manufacturing Operations, IT, Finance

**Approval Criteria:**
- Business requirements are clear and complete
- User stories have testable acceptance criteria
- Success metrics are measurable and realistic
- Dependencies and constraints are documented
- Rollout strategy is feasible

Once approved, this document will be handed off to the Architecture team for technical design.

---

**End of Product Requirements Document**
