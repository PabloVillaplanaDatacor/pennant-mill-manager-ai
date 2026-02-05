# Navigation System - Technical Architecture & Implementation Plan

## Document Information

| Field | Value |
|-------|-------|
| **Feature Module** | Navigation System |
| **Document Owner** | Senior Software Architect |
| **Created Date** | 2026-02-05 |
| **Status** | Ready for Development |
| **Version** | 1.0 |
| **Based on PRD** | navigation-system/product-requirements.md v1.0 |

---

## Table of Contents

1. [Architecture Overview](#1-architecture-overview)
2. [Current Codebase Analysis](#2-current-codebase-analysis)
3. [Component Architecture](#3-component-architecture)
4. [State Management Strategy](#4-state-management-strategy)
5. [Routing Integration](#5-routing-integration)
6. [Real-Time Notification Architecture](#6-real-time-notification-architecture)
7. [Accessibility Implementation](#7-accessibility-implementation)
8. [Performance Optimization Strategy](#8-performance-optimization-strategy)
9. [Mobile Responsiveness Strategy](#9-mobile-responsiveness-strategy)
10. [Technical Task Breakdown](#10-technical-task-breakdown)
11. [Data Architecture](#11-data-architecture)
12. [API & Integration Design](#12-api--integration-design)
13. [Testing Strategy](#13-testing-strategy)
14. [Risk Assessment & Mitigation](#14-risk-assessment--mitigation)
15. [Implementation Phasing](#15-implementation-phasing)

---

## 1. Architecture Overview

### 1.1 Architectural Approach

**Architecture Pattern**: Component-Based Architecture with Clear Separation of Concerns

The Navigation System follows a layered component architecture aligned with Next.js 16 App Router best practices:

```
┌─────────────────────────────────────────────────────────┐
│                    Presentation Layer                    │
│  - Navigation Components (Sidebar, TopBar, Mobile)      │
│  - UI Components (Badges, Dropdowns, Icons)             │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                    Application Layer                     │
│  - Navigation State Management (Context/Hooks)          │
│  - Keyboard Shortcut Handlers                           │
│  - Route Change Detection                               │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                    Domain Layer                          │
│  - Navigation Configuration                             │
│  - User Preferences                                     │
│  - Notification Business Logic                          │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│                    Integration Layer                     │
│  - API Client (Notifications, User Profile)             │
│  - WebSocket Connection Manager                         │
│  - LocalStorage Adapter                                 │
└─────────────────────────────────────────────────────────┘
```

### 1.2 Technology Stack

**Core Technologies** (Already in codebase):
- **Next.js 16**: App Router for server-side rendering and routing
- **React 19**: Component architecture and hooks
- **TypeScript 5**: Type safety and developer experience
- **Tailwind CSS 4**: Utility-first styling

**Additional Dependencies** (To be added):
- **Lucide React**: Icon library (consistent, tree-shakeable, 24px icons)
- **@radix-ui/react-dropdown-menu**: Accessible dropdown components
- **@radix-ui/react-dialog**: Accessible modal/dialog for confirmations
- **react-hot-toast**: Toast notifications for feedback
- **zustand**: Lightweight state management (3KB, simpler than Redux)
- **socket.io-client**: WebSocket client for real-time notifications
- **@tanstack/react-query**: Data fetching and caching for API calls

### 1.3 Feature Placement in Architecture

The Navigation System is a **foundational cross-cutting concern** that:
- Lives in `src/components/navigation/` (shared components)
- Integrates with root layout `src/app/layout.tsx`
- Provides context to all pages via React Context
- Does NOT belong to any single business module

### 1.4 Data Flow

```
User Interaction
    ↓
Navigation Component (Sidebar/TopBar)
    ↓
Navigation Context (State Update)
    ↓
Next.js Router (route.push)
    ↓
Server-Side Page Load
    ↓
Active State Recalculation (derived from pathname)
    ↓
UI Update (Active Highlight)
```

**Real-Time Notification Flow:**
```
Backend Alert Created
    ↓
WebSocket Server Broadcast
    ↓
WebSocket Client (socket.io)
    ↓
Notification Context (State Update)
    ↓
Badge Component Re-render
```

### 1.5 Security Model

**Authentication & Authorization**:
- Navigation components receive user session from Next.js middleware
- User permissions checked server-side before rendering restricted routes
- Client-side navigation items filtered based on user role
- Session token validated on every API call (HttpOnly cookies)

**Data Security**:
- No sensitive data in localStorage (only UI preferences)
- API endpoints require authentication
- WebSocket connection authenticated via session token
- XSS prevention via React's built-in sanitization

### 1.6 Performance Strategy

**Performance Targets**:
- Time to Interactive (TTI): < 1.5 seconds
- Active state change: < 100ms
- Sidebar animation: 250-300ms
- Navigation to new page: < 3 seconds (p80)

**Optimization Techniques**:
- Server-side rendering for initial navigation state
- Prefetching critical routes (Dashboard, Inventory)
- Icon sprite sheet for faster loading
- Debounced WebSocket reconnection
- Lazy-loaded profile dropdown content

---

## 2. Current Codebase Analysis

### 2.1 Existing Architecture

**Current Project Structure**:
```
pennant-mill-manage-ai/
├── src/
│   └── app/
│       ├── layout.tsx          # Root layout (Geist fonts, metadata)
│       ├── page.tsx            # Current landing page
│       └── globals.css         # Tailwind imports, CSS variables
├── package.json                # Next.js 16, React 19, Tailwind 4
├── tsconfig.json              # TypeScript strict mode enabled
└── CLAUDE.md                  # Project instructions
```

**Key Observations**:

1. **Root Layout** (`src/app/layout.tsx`):
   - Currently minimal setup with Geist fonts
   - No navigation structure yet
   - Ready for navigation component integration
   - Uses `<html lang="en">` (will need i18n support for Spanish)

2. **Current Page** (`src/app/page.tsx`):
   - Simple landing page (Datacor presentation)
   - Will be replaced with Dashboard module
   - No existing navigation

3. **Styling** (`src/app/globals.css`):
   - Tailwind CSS 4 configured
   - CSS custom properties for theming
   - Dark mode support via `prefers-color-scheme`
   - Will need PMM brand colors added

4. **TypeScript Configuration**:
   - Strict mode enabled (good for type safety)
   - Path aliases configured (`@/*` → `src/*`)
   - ES2017 target (supports modern browsers)

5. **No Existing State Management**:
   - Clean slate for navigation state
   - Will implement Zustand for simplicity

### 2.2 Architectural Gaps to Address

**Missing Components**:
- [ ] Navigation component structure
- [ ] Sidebar component
- [ ] Top bar component
- [ ] Mobile overlay/drawer
- [ ] Notification badge component
- [ ] User profile dropdown

**Missing State Management**:
- [ ] Navigation state (sidebar collapsed/expanded)
- [ ] Active route detection
- [ ] Notification count state
- [ ] User session state

**Missing Integrations**:
- [ ] Backend API client
- [ ] WebSocket connection
- [ ] localStorage wrapper
- [ ] Authentication middleware

**Missing Accessibility**:
- [ ] ARIA labels and roles
- [ ] Keyboard navigation handlers
- [ ] Screen reader announcements
- [ ] Focus management

### 2.3 Compatibility Considerations

**Next.js 16 App Router Specifics**:
- Use `usePathname()` hook for active route detection
- Implement navigation in Server Components where possible
- Use Client Components (`'use client'`) only where needed (interactive elements)
- Leverage automatic code splitting per route

**React 19 Features**:
- Use new `use()` hook for context consumption (simpler than `useContext`)
- Leverage automatic batching for state updates
- Use new Server Components architecture

**Tailwind CSS 4**:
- Use new `@theme` directive for custom properties
- Leverage improved dark mode utilities
- Use container queries for responsive navigation

---

## 3. Component Architecture

### 3.1 Component Hierarchy

```
AppLayout (src/app/layout.tsx)
├── NavigationProvider (context)
│   ├── Sidebar (Desktop)
│   │   ├── SidebarLogo
│   │   ├── SidebarNav
│   │   │   └── NavItem (x6 modules)
│   │   └── SidebarCollapseButton
│   ├── MobileNav (Mobile/Tablet)
│   │   ├── HamburgerButton
│   │   └── MobileDrawer
│   │       ├── SidebarLogo
│   │       ├── SidebarNav
│   │       └── CloseButton
│   └── TopBar
│       ├── AppTitle
│       ├── NotificationBadge
│       │   └── NotificationDropdown (future)
│       └── UserProfileMenu
│           └── ProfileDropdown
└── main content (children)
```

### 3.2 Component Specifications

#### 3.2.1 `NavigationProvider`

**Purpose**: Centralized state management for navigation

**Responsibilities**:
- Manage sidebar collapsed/expanded state
- Manage mobile drawer open/closed state
- Provide navigation configuration
- Handle keyboard shortcuts globally
- Sync state with localStorage

**Implementation**:
```typescript
// src/components/navigation/NavigationProvider.tsx
'use client';

import { createContext, useContext, useState, useEffect } from 'react';
import { usePathname } from 'next/navigation';

interface NavigationContextType {
  isSidebarCollapsed: boolean;
  toggleSidebar: () => void;
  isMobileDrawerOpen: boolean;
  setMobileDrawerOpen: (open: boolean) => void;
  activeRoute: string;
  navigationItems: NavigationItem[];
}

const NavigationContext = createContext<NavigationContextType | null>(null);

export function NavigationProvider({ children }: { children: React.ReactNode }) {
  const pathname = usePathname();
  const [isSidebarCollapsed, setIsSidebarCollapsed] = useState(false);
  const [isMobileDrawerOpen, setMobileDrawerOpen] = useState(false);

  // Load collapsed state from localStorage
  useEffect(() => {
    const saved = localStorage.getItem('pmm_sidebar_collapsed');
    if (saved !== null) {
      setIsSidebarCollapsed(saved === 'true');
    }
  }, []);

  // Save collapsed state to localStorage
  const toggleSidebar = () => {
    setIsSidebarCollapsed(prev => {
      const newState = !prev;
      localStorage.setItem('pmm_sidebar_collapsed', String(newState));
      return newState;
    });
  };

  // Determine active route
  const activeRoute = determineActiveRoute(pathname);

  // Navigation configuration
  const navigationItems = getNavigationConfig();

  return (
    <NavigationContext.Provider
      value={{
        isSidebarCollapsed,
        toggleSidebar,
        isMobileDrawerOpen,
        setMobileDrawerOpen,
        activeRoute,
        navigationItems,
      }}
    >
      {children}
    </NavigationContext.Provider>
  );
}

export function useNavigation() {
  const context = useContext(NavigationContext);
  if (!context) {
    throw new Error('useNavigation must be used within NavigationProvider');
  }
  return context;
}
```

**Type Safety**:
```typescript
// src/types/navigation.ts
export interface NavigationItem {
  id: string;
  label: string;
  labelEn: string;
  route: string;
  icon: React.ComponentType<{ className?: string }>;
  shortcut: string;
  shortcutMac: string;
}
```

#### 3.2.2 `Sidebar`

**Purpose**: Primary desktop navigation

**Props**:
```typescript
interface SidebarProps {
  className?: string;
}
```

**Responsibilities**:
- Render navigation items
- Handle active state styling
- Animate collapse/expand
- Handle keyboard navigation

**Implementation Strategy**:
```typescript
// src/components/navigation/Sidebar.tsx
'use client';

import { useNavigation } from './NavigationProvider';
import { SidebarLogo } from './SidebarLogo';
import { SidebarNav } from './SidebarNav';
import { SidebarCollapseButton } from './SidebarCollapseButton';

export function Sidebar({ className }: SidebarProps) {
  const { isSidebarCollapsed } = useNavigation();

  return (
    <aside
      className={cn(
        'fixed left-0 top-0 h-screen bg-slate-800 transition-all duration-300',
        isSidebarCollapsed ? 'w-16' : 'w-64',
        'hidden md:flex md:flex-col',
        className
      )}
      aria-label="Main navigation"
    >
      <SidebarLogo collapsed={isSidebarCollapsed} />
      <SidebarNav collapsed={isSidebarCollapsed} />
      <SidebarCollapseButton collapsed={isSidebarCollapsed} />
    </aside>
  );
}
```

**Styling Specifications**:
- Expanded width: `16rem` (256px)
- Collapsed width: `4rem` (64px)
- Background: `#1E3A5F` (dark blue)
- Z-index: `40` (below modals, above content)
- Transition: `width 300ms cubic-bezier(0.4, 0.0, 0.2, 1)`

#### 3.2.3 `NavItem`

**Purpose**: Individual navigation link

**Props**:
```typescript
interface NavItemProps {
  item: NavigationItem;
  isActive: boolean;
  isCollapsed: boolean;
  onClick?: () => void;
}
```

**Responsibilities**:
- Render icon and label
- Apply active state styling
- Handle click navigation
- Show tooltip in collapsed mode
- Announce state to screen readers

**Implementation Strategy**:
```typescript
// src/components/navigation/NavItem.tsx
'use client';

import Link from 'next/link';
import { useRouter } from 'next/navigation';
import * as Tooltip from '@radix-ui/react-tooltip';

export function NavItem({ item, isActive, isCollapsed, onClick }: NavItemProps) {
  const router = useRouter();
  const Icon = item.icon;

  const handleClick = (e: React.MouseEvent) => {
    // Prevent navigation if already on active route
    if (isActive) {
      e.preventDefault();
      return;
    }
    onClick?.();
  };

  const navLink = (
    <Link
      href={item.route}
      onClick={handleClick}
      className={cn(
        'flex items-center gap-3 px-4 py-3 rounded-lg transition-colors',
        'focus-visible:outline focus-visible:outline-2 focus-visible:outline-orange-500',
        isActive
          ? 'bg-orange-500 text-white'
          : 'text-white hover:bg-white/10',
        isCollapsed && 'justify-center px-2'
      )}
      aria-current={isActive ? 'page' : undefined}
    >
      <Icon className="w-6 h-6 flex-shrink-0" aria-hidden="true" />
      {!isCollapsed && <span className="font-medium">{item.label}</span>}
    </Link>
  );

  // Wrap in tooltip for collapsed mode
  if (isCollapsed) {
    return (
      <Tooltip.Provider>
        <Tooltip.Root>
          <Tooltip.Trigger asChild>{navLink}</Tooltip.Trigger>
          <Tooltip.Portal>
            <Tooltip.Content
              side="right"
              className="bg-slate-900 text-white px-3 py-2 rounded text-sm shadow-lg"
              sideOffset={5}
            >
              {item.label}
              <span className="text-slate-400 ml-2">({item.shortcut})</span>
              <Tooltip.Arrow className="fill-slate-900" />
            </Tooltip.Content>
          </Tooltip.Portal>
        </Tooltip.Root>
      </Tooltip.Provider>
    );
  }

  return navLink;
}
```

#### 3.2.4 `TopBar`

**Purpose**: Top navigation bar with notifications and profile

**Implementation Strategy**:
```typescript
// src/components/navigation/TopBar.tsx
'use client';

import { NotificationBadge } from './NotificationBadge';
import { UserProfileMenu } from './UserProfileMenu';
import { MobileMenuButton } from './MobileMenuButton';

export function TopBar() {
  return (
    <header className="fixed top-0 right-0 left-0 md:left-64 h-16 bg-white border-b border-gray-200 z-30 transition-all duration-300">
      <div className="flex items-center justify-between h-full px-4 md:px-6">
        <div className="flex items-center gap-4">
          <MobileMenuButton className="md:hidden" />
          <h1 className="text-lg font-semibold text-gray-900">
            Pennant Mill Manager
          </h1>
        </div>
        <div className="flex items-center gap-4">
          <NotificationBadge />
          <UserProfileMenu />
        </div>
      </div>
    </header>
  );
}
```

#### 3.2.5 `NotificationBadge`

**Purpose**: Display unread notification count

**Implementation Strategy**:
```typescript
// src/components/navigation/NotificationBadge.tsx
'use client';

import { Bell } from 'lucide-react';
import { useNotifications } from '@/hooks/useNotifications';
import Link from 'next/link';

export function NotificationBadge() {
  const { unreadCount, isLoading } = useNotifications();

  return (
    <Link
      href="/alerts"
      className="relative p-2 rounded-lg hover:bg-gray-100 transition-colors focus-visible:outline focus-visible:outline-2 focus-visible:outline-orange-500"
      aria-label={`${unreadCount} unread notifications`}
    >
      <Bell className="w-6 h-6 text-gray-700" />
      {unreadCount > 0 && (
        <span className="absolute -top-1 -right-1 flex items-center justify-center min-w-[20px] h-5 px-1 text-xs font-bold text-white bg-red-500 rounded-full border-2 border-white">
          {unreadCount > 9 ? '9+' : unreadCount}
        </span>
      )}
    </Link>
  );
}
```

#### 3.2.6 `MobileDrawer`

**Purpose**: Overlay navigation for mobile/tablet

**Implementation Strategy**:
```typescript
// src/components/navigation/MobileDrawer.tsx
'use client';

import { useNavigation } from './NavigationProvider';
import { X } from 'lucide-react';
import { SidebarNav } from './SidebarNav';
import { SidebarLogo } from './SidebarLogo';
import { useEffect } from 'react';

export function MobileDrawer() {
  const { isMobileDrawerOpen, setMobileDrawerOpen } = useNavigation();

  // Lock body scroll when drawer is open
  useEffect(() => {
    if (isMobileDrawerOpen) {
      document.body.style.overflow = 'hidden';
    } else {
      document.body.style.overflow = '';
    }
    return () => {
      document.body.style.overflow = '';
    };
  }, [isMobileDrawerOpen]);

  if (!isMobileDrawerOpen) return null;

  return (
    <>
      {/* Backdrop */}
      <div
        className="fixed inset-0 bg-black/50 z-40 md:hidden animate-fade-in"
        onClick={() => setMobileDrawerOpen(false)}
        aria-hidden="true"
      />

      {/* Drawer */}
      <aside
        className="fixed top-0 left-0 h-screen w-80 bg-slate-800 z-50 md:hidden animate-slide-in-left"
        role="dialog"
        aria-label="Mobile navigation"
      >
        <div className="flex flex-col h-full">
          <div className="flex items-center justify-between p-4 border-b border-slate-700">
            <SidebarLogo collapsed={false} />
            <button
              onClick={() => setMobileDrawerOpen(false)}
              className="p-2 rounded-lg text-white hover:bg-white/10"
              aria-label="Close navigation"
            >
              <X className="w-6 h-6" />
            </button>
          </div>
          <SidebarNav
            collapsed={false}
            onItemClick={() => setMobileDrawerOpen(false)}
          />
        </div>
      </aside>
    </>
  );
}
```

### 3.3 Component File Structure

```
src/
├── components/
│   └── navigation/
│       ├── NavigationProvider.tsx      # Context provider
│       ├── Sidebar.tsx                 # Desktop sidebar
│       ├── SidebarLogo.tsx            # Logo component
│       ├── SidebarNav.tsx             # Navigation list
│       ├── NavItem.tsx                # Individual nav item
│       ├── SidebarCollapseButton.tsx  # Collapse/expand button
│       ├── MobileDrawer.tsx           # Mobile overlay
│       ├── MobileMenuButton.tsx       # Hamburger button
│       ├── TopBar.tsx                 # Top navigation bar
│       ├── NotificationBadge.tsx      # Notification badge
│       ├── UserProfileMenu.tsx        # Profile dropdown
│       ├── ProfileDropdown.tsx        # Dropdown content
│       └── index.ts                   # Barrel export
├── hooks/
│   ├── useNotifications.ts            # Notification data hook
│   ├── useKeyboardShortcuts.ts        # Keyboard handler
│   └── useActiveRoute.ts              # Active route detection
├── lib/
│   ├── navigation-config.ts           # Navigation items config
│   └── api/
│       ├── notifications.ts           # Notification API client
│       └── user.ts                    # User API client
└── types/
    └── navigation.ts                  # TypeScript types
```

---

## 4. State Management Strategy

### 4.1 State Architecture Decision

**Chosen Approach: Zustand + React Context**

**Rationale**:
- **Zustand** for global navigation state (sidebar collapsed, notifications)
- **React Context** for providing state to nested components
- **Next.js URL state** for active route (source of truth)
- **LocalStorage** for persistence

**Why Not Redux**:
- Zustand is 3KB vs Redux 45KB (smaller bundle)
- Simpler API, less boilerplate
- No need for complex middleware for this use case

**Why Not Pure Context**:
- Context alone lacks devtools and middleware
- Zustand provides better performance for frequent updates

### 4.2 State Slice Design

```typescript
// src/store/navigation.ts
import { create } from 'zustand';
import { persist, createJSONStorage } from 'zustand/middleware';

interface NavigationState {
  // Sidebar state
  isSidebarCollapsed: boolean;
  toggleSidebar: () => void;
  setSidebarCollapsed: (collapsed: boolean) => void;

  // Mobile drawer state
  isMobileDrawerOpen: boolean;
  setMobileDrawerOpen: (open: boolean) => void;

  // Notification state
  unreadNotificationCount: number;
  setUnreadNotificationCount: (count: number) => void;

  // User profile state
  user: UserProfile | null;
  setUser: (user: UserProfile | null) => void;
}

export const useNavigationStore = create<NavigationState>()(
  persist(
    (set) => ({
      // Sidebar
      isSidebarCollapsed: false,
      toggleSidebar: () =>
        set((state) => ({ isSidebarCollapsed: !state.isSidebarCollapsed })),
      setSidebarCollapsed: (collapsed) =>
        set({ isSidebarCollapsed: collapsed }),

      // Mobile drawer
      isMobileDrawerOpen: false,
      setMobileDrawerOpen: (open) => set({ isMobileDrawerOpen: open }),

      // Notifications
      unreadNotificationCount: 0,
      setUnreadNotificationCount: (count) =>
        set({ unreadNotificationCount: count }),

      // User
      user: null,
      setUser: (user) => set({ user }),
    }),
    {
      name: 'pmm-navigation',
      storage: createJSONStorage(() => localStorage),
      partialize: (state) => ({
        // Only persist sidebar collapsed state
        isSidebarCollapsed: state.isSidebarCollapsed,
      }),
    }
  )
);
```

### 4.3 State Update Patterns

**Sidebar Toggle**:
```typescript
// User clicks collapse button
const { toggleSidebar } = useNavigationStore();
toggleSidebar(); // State updates, localStorage syncs automatically
```

**Notification Update (WebSocket)**:
```typescript
// WebSocket receives new notification
socket.on('notification:new', (data) => {
  const { setUnreadNotificationCount } = useNavigationStore.getState();
  setUnreadNotificationCount(data.unreadCount);
});
```

**Active Route Detection**:
```typescript
// Derived from Next.js pathname (no state needed)
const pathname = usePathname();
const activeRoute = determineActiveRoute(pathname);
```

### 4.4 Server-Side State Hydration

**Challenge**: Prevent flash of incorrect state on page load

**Solution**: Server-side render with default state, hydrate client-side

```typescript
// src/app/layout.tsx
export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="es">
      <body>
        <NavigationProvider>
          <NavigationLayout>{children}</NavigationLayout>
        </NavigationProvider>
      </body>
    </html>
  );
}

// src/components/navigation/NavigationLayout.tsx (Client Component)
'use client';

import { useEffect } from 'react';
import { useNavigationStore } from '@/store/navigation';

export function NavigationLayout({ children }: { children: React.ReactNode }) {
  const [isHydrated, setIsHydrated] = useState(false);

  useEffect(() => {
    setIsHydrated(true);
  }, []);

  // Prevent flash of incorrect sidebar state
  if (!isHydrated) {
    return (
      <>
        <Sidebar className="opacity-0" />
        <TopBar />
        <main className="pt-16 md:pl-64">{children}</main>
      </>
    );
  }

  return (
    <>
      <Sidebar />
      <MobileDrawer />
      <TopBar />
      <main className="pt-16 md:pl-64 transition-all duration-300">
        {children}
      </main>
    </>
  );
}
```

---

## 5. Routing Integration

### 5.1 Next.js App Router Integration

**Route Structure**:
```
src/app/
├── layout.tsx                 # Root layout with navigation
├── page.tsx                   # Dashboard (/)
├── inventory/
│   ├── layout.tsx             # Inventory module layout
│   └── page.tsx               # Inventory list
├── categories/
│   └── page.tsx               # Categories management
├── alerts/
│   └── page.tsx               # Alerts dashboard
├── reports/
│   └── page.tsx               # Reports module
└── configuration/
    └── page.tsx               # System configuration
```

### 5.2 Active Route Detection Logic

```typescript
// src/lib/navigation-utils.ts
export function determineActiveRoute(pathname: string): string {
  // Exact match for root
  if (pathname === '/') return '/';

  // Extract first segment for module matching
  const segments = pathname.split('/').filter(Boolean);
  const moduleSegment = segments[0];

  // Map to navigation route
  const routeMap: Record<string, string> = {
    'inventory': '/inventory',
    'inventario': '/inventory',
    'categories': '/categories',
    'categorias': '/categories',
    'alerts': '/alerts',
    'alertas': '/alerts',
    'reports': '/reports',
    'reportes': '/reports',
    'configuration': '/configuration',
    'configuracion': '/configuration',
  };

  return routeMap[moduleSegment] || '/';
}
```

### 5.3 Navigation Configuration

```typescript
// src/lib/navigation-config.ts
import {
  LayoutDashboard,
  Package,
  FolderTree,
  AlertTriangle,
  FileBarChart,
  Settings,
} from 'lucide-react';
import { NavigationItem } from '@/types/navigation';

export const NAVIGATION_ITEMS: NavigationItem[] = [
  {
    id: 'dashboard',
    label: 'Dashboard',
    labelEn: 'Dashboard',
    route: '/',
    icon: LayoutDashboard,
    shortcut: 'Alt+D',
    shortcutMac: 'Cmd+D',
  },
  {
    id: 'inventory',
    label: 'Inventario',
    labelEn: 'Inventory',
    route: '/inventory',
    icon: Package,
    shortcut: 'Alt+I',
    shortcutMac: 'Cmd+I',
  },
  {
    id: 'categories',
    label: 'Categorías',
    labelEn: 'Categories',
    route: '/categories',
    icon: FolderTree,
    shortcut: 'Alt+C',
    shortcutMac: 'Cmd+C',
  },
  {
    id: 'alerts',
    label: 'Alertas',
    labelEn: 'Alerts',
    route: '/alerts',
    icon: AlertTriangle,
    shortcut: 'Alt+A',
    shortcutMac: 'Cmd+A',
  },
  {
    id: 'reports',
    label: 'Reportes',
    labelEn: 'Reports',
    route: '/reports',
    icon: FileBarChart,
    shortcut: 'Alt+R',
    shortcutMac: 'Cmd+R',
  },
  {
    id: 'configuration',
    label: 'Configuración',
    labelEn: 'Configuration',
    route: '/configuration',
    icon: Settings,
    shortcut: 'Alt+S',
    shortcutMac: 'Cmd+S',
  },
];
```

### 5.4 Programmatic Navigation

```typescript
// Using Next.js router
import { useRouter } from 'next/navigation';

function NavItem({ item }: { item: NavigationItem }) {
  const router = useRouter();

  const handleClick = () => {
    // Optimistic UI update
    router.push(item.route);
  };

  return <button onClick={handleClick}>...</button>;
}
```

### 5.5 Route Prefetching Strategy

```typescript
// Prefetch critical routes on hover
<Link
  href={item.route}
  prefetch={item.id === 'dashboard' || item.id === 'inventory'}
  onMouseEnter={() => {
    // Prefetch on hover for faster navigation
    router.prefetch(item.route);
  }}
>
  {item.label}
</Link>
```

---

## 6. Real-Time Notification Architecture

### 6.1 WebSocket Architecture

**Technology Choice: Socket.IO Client**

**Rationale**:
- Auto-reconnection with exponential backoff
- Fallback to polling if WebSocket unavailable
- Built-in event-based messaging
- Cross-browser compatibility

**Alternative Considered**: Native WebSocket
- Requires manual reconnection logic
- No automatic fallback mechanism
- More complex error handling

### 6.2 WebSocket Connection Manager

```typescript
// src/lib/websocket/socket-manager.ts
import { io, Socket } from 'socket.io-client';
import { useNavigationStore } from '@/store/navigation';

class SocketManager {
  private socket: Socket | null = null;
  private reconnectAttempts = 0;
  private maxReconnectAttempts = 5;

  connect(userId: string, sessionToken: string) {
    if (this.socket?.connected) return;

    this.socket = io(process.env.NEXT_PUBLIC_WS_URL || 'ws://localhost:3001', {
      auth: { token: sessionToken },
      query: { userId },
      transports: ['websocket', 'polling'],
      reconnectionDelay: 1000,
      reconnectionDelayMax: 5000,
    });

    this.setupListeners();
  }

  private setupListeners() {
    if (!this.socket) return;

    // Connection events
    this.socket.on('connect', () => {
      console.log('WebSocket connected');
      this.reconnectAttempts = 0;
    });

    this.socket.on('disconnect', (reason) => {
      console.log('WebSocket disconnected:', reason);
      if (reason === 'io server disconnect') {
        // Server initiated disconnect, attempt manual reconnect
        this.socket?.connect();
      }
    });

    this.socket.on('connect_error', (error) => {
      console.error('WebSocket connection error:', error);
      this.reconnectAttempts++;

      if (this.reconnectAttempts >= this.maxReconnectAttempts) {
        console.warn('Max reconnect attempts reached, falling back to polling');
        // Trigger polling fallback
        this.disconnect();
      }
    });

    // Notification events
    this.socket.on('notification:new', (data: { unreadCount: number }) => {
      const { setUnreadNotificationCount } = useNavigationStore.getState();
      setUnreadNotificationCount(data.unreadCount);
    });

    this.socket.on('notification:read', (data: { unreadCount: number }) => {
      const { setUnreadNotificationCount } = useNavigationStore.getState();
      setUnreadNotificationCount(data.unreadCount);
    });
  }

  disconnect() {
    this.socket?.disconnect();
    this.socket = null;
  }

  isConnected(): boolean {
    return this.socket?.connected || false;
  }
}

export const socketManager = new SocketManager();
```

### 6.3 Polling Fallback Mechanism

```typescript
// src/hooks/useNotifications.ts
import { useQuery } from '@tanstack/react-query';
import { useEffect, useState } from 'react';
import { socketManager } from '@/lib/websocket/socket-manager';
import { fetchNotificationSummary } from '@/lib/api/notifications';

export function useNotifications() {
  const [usePolling, setUsePolling] = useState(false);

  // Check WebSocket connection status
  useEffect(() => {
    const checkInterval = setInterval(() => {
      if (!socketManager.isConnected()) {
        setUsePolling(true);
      } else {
        setUsePolling(false);
      }
    }, 10000); // Check every 10 seconds

    return () => clearInterval(checkInterval);
  }, []);

  // Polling fallback (30-second interval)
  const { data: pollingData } = useQuery({
    queryKey: ['notifications', 'summary'],
    queryFn: fetchNotificationSummary,
    refetchInterval: usePolling ? 30000 : false, // 30 seconds
    enabled: usePolling,
  });

  // Get count from Zustand store (updated by WebSocket or polling)
  const unreadCount = useNavigationStore((state) => state.unreadNotificationCount);

  // Update store when polling data changes
  useEffect(() => {
    if (usePolling && pollingData) {
      useNavigationStore.getState().setUnreadNotificationCount(pollingData.unreadCount);
    }
  }, [pollingData, usePolling]);

  return {
    unreadCount,
    isLoading: false,
    isPolling: usePolling,
  };
}
```

### 6.4 Connection Lifecycle

```typescript
// src/components/navigation/NavigationProvider.tsx
'use client';

import { useEffect } from 'react';
import { socketManager } from '@/lib/websocket/socket-manager';
import { useSession } from '@/hooks/useSession';

export function NavigationProvider({ children }: { children: React.ReactNode }) {
  const { user, sessionToken } = useSession();

  // Connect WebSocket when user is authenticated
  useEffect(() => {
    if (user && sessionToken) {
      socketManager.connect(user.id, sessionToken);
    }

    return () => {
      socketManager.disconnect();
    };
  }, [user, sessionToken]);

  return (
    // ... provider implementation
  );
}
```

### 6.5 Error Handling & User Feedback

```typescript
// Show user-friendly message when WebSocket fails
import { toast } from 'react-hot-toast';

socket.on('connect_error', () => {
  if (reconnectAttempts === maxReconnectAttempts - 1) {
    toast.error(
      'Actualizaciones en tiempo real no disponibles. Usando modo de actualización periódica.',
      { duration: 5000 }
    );
  }
});
```

---

## 7. Accessibility Implementation

### 7.1 WCAG 2.1 AA Compliance Strategy

**Critical Accessibility Requirements**:
- Keyboard navigation (100% functionality)
- Screen reader support (ARIA labels)
- Color contrast (4.5:1 text, 3:1 icons)
- Touch targets (44x44px minimum)
- Focus indicators (visible outline)

### 7.2 Semantic HTML Structure

```typescript
// Correct semantic structure
<nav aria-label="Main navigation">
  <ul role="list">
    <li>
      <a href="/" aria-current="page">Dashboard</a>
    </li>
    <li>
      <a href="/inventory">Inventario</a>
    </li>
  </ul>
</nav>

// Top bar
<header role="banner">
  <h1>Pennant Mill Manager</h1>
  <nav aria-label="User navigation">...</nav>
</header>

// Main content
<main id="main-content" role="main">
  {children}
</main>
```

### 7.3 ARIA Labels & Roles

```typescript
// Sidebar navigation
<aside
  role="navigation"
  aria-label="Main navigation"
  aria-expanded={!isSidebarCollapsed}
>
  <nav>
    <ul role="list">
      {navigationItems.map((item) => (
        <li key={item.id}>
          <Link
            href={item.route}
            aria-current={isActive ? 'page' : undefined}
            aria-label={`Navigate to ${item.label}`}
          >
            <Icon aria-hidden="true" />
            <span>{item.label}</span>
          </Link>
        </li>
      ))}
    </ul>
  </nav>
</aside>

// Notification badge
<button
  aria-label={`${unreadCount} unread notifications. Click to view alerts.`}
  aria-describedby="notification-count"
>
  <Bell aria-hidden="true" />
  <span id="notification-count" className="sr-only">
    {unreadCount} unread
  </span>
  <span aria-hidden="true" className="badge">
    {unreadCount > 9 ? '9+' : unreadCount}
  </span>
</button>

// Collapse button
<button
  aria-label={isSidebarCollapsed ? 'Expand navigation' : 'Collapse navigation'}
  aria-expanded={!isSidebarCollapsed}
  aria-controls="main-navigation"
>
  {isSidebarCollapsed ? <ChevronRight /> : <ChevronLeft />}
</button>

// Mobile drawer
<div
  role="dialog"
  aria-label="Mobile navigation menu"
  aria-modal="true"
  hidden={!isMobileDrawerOpen}
>
  {/* Navigation content */}
</div>
```

### 7.4 Keyboard Navigation Implementation

```typescript
// src/hooks/useKeyboardShortcuts.ts
import { useEffect } from 'react';
import { useRouter } from 'next/navigation';
import { NAVIGATION_ITEMS } from '@/lib/navigation-config';

export function useKeyboardShortcuts() {
  const router = useRouter();

  useEffect(() => {
    function handleKeyDown(event: KeyboardEvent) {
      // Ignore shortcuts when typing in input fields
      const target = event.target as HTMLElement;
      if (
        target.tagName === 'INPUT' ||
        target.tagName === 'TEXTAREA' ||
        target.isContentEditable
      ) {
        return;
      }

      // Detect platform (Mac vs Windows/Linux)
      const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
      const modifierKey = isMac ? event.metaKey : event.altKey;

      // Check if modifier key is pressed
      if (!modifierKey) return;

      // Map shortcuts to navigation items
      NAVIGATION_ITEMS.forEach((item) => {
        const shortcutKey = isMac
          ? item.shortcutMac.split('+')[1].toLowerCase()
          : item.shortcut.split('+')[1].toLowerCase();

        if (event.key.toLowerCase() === shortcutKey) {
          event.preventDefault();
          router.push(item.route);

          // Announce navigation to screen readers
          announceToScreenReader(`Navigating to ${item.label}`);
        }
      });

      // Alt+/ or Cmd+/ for keyboard shortcut help
      if (event.key === '/' || event.key === '?') {
        event.preventDefault();
        // Open keyboard shortcuts modal
      }

      // Escape to close dropdowns/modals
      if (event.key === 'Escape') {
        // Close any open dropdowns
        useNavigationStore.getState().setMobileDrawerOpen(false);
      }
    }

    window.addEventListener('keydown', handleKeyDown);
    return () => window.removeEventListener('keydown', handleKeyDown);
  }, [router]);
}

// Announce to screen readers
function announceToScreenReader(message: string) {
  const announcement = document.createElement('div');
  announcement.setAttribute('role', 'status');
  announcement.setAttribute('aria-live', 'polite');
  announcement.setAttribute('aria-atomic', 'true');
  announcement.className = 'sr-only';
  announcement.textContent = message;
  document.body.appendChild(announcement);

  setTimeout(() => {
    document.body.removeChild(announcement);
  }, 1000);
}
```

### 7.5 Focus Management

```typescript
// Skip to main content link
export function SkipLink() {
  return (
    <a
      href="#main-content"
      className="sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 focus:z-50 focus:px-4 focus:py-2 focus:bg-orange-500 focus:text-white focus:rounded"
    >
      Skip to main content
    </a>
  );
}

// Focus trap for mobile drawer
import { useFocusTrap } from '@/hooks/useFocusTrap';

export function MobileDrawer() {
  const drawerRef = useRef<HTMLDivElement>(null);
  const { isMobileDrawerOpen } = useNavigation();

  useFocusTrap(drawerRef, isMobileDrawerOpen);

  return (
    <div ref={drawerRef} role="dialog" aria-modal="true">
      {/* Navigation content */}
    </div>
  );
}
```

### 7.6 Color Contrast Verification

**Color Palette**:
```typescript
// src/lib/theme-colors.ts
export const PMM_COLORS = {
  // Primary
  orange: {
    500: '#F97316', // Active state
    600: '#EA580C', // Hover/pressed
  },

  // Neutral
  slate: {
    800: '#1E293B', // Sidebar background (adjusted for contrast)
    900: '#0F172A', // Tooltip background
  },

  // Text
  white: '#FFFFFF',       // Contrast ratio 12.63:1 on slate-800
  gray: {
    700: '#374151',       // Body text on white
    600: '#4B5563',       // Secondary text
  },

  // Status
  red: {
    500: '#EF4444',       // Error/notification badge
  },
};

// Verify contrast ratios meet WCAG AA
// - White text on slate-800: 12.63:1 ✓ (exceeds 4.5:1)
// - White text on orange-500: 4.52:1 ✓ (meets 4.5:1)
// - Gray-700 on white: 10.07:1 ✓ (exceeds 4.5:1)
```

### 7.7 Screen Reader Testing Checklist

```markdown
## Screen Reader Testing (NVDA/JAWS/VoiceOver)

- [ ] Navigation landmark announced as "Main navigation"
- [ ] Active page announced with "current page"
- [ ] Notification badge count announced correctly
- [ ] Sidebar collapse/expand state announced
- [ ] Keyboard shortcuts announced on trigger
- [ ] Mobile drawer open/close announced
- [ ] Focus moves logically (top to bottom, left to right)
- [ ] No keyboard traps (can always escape with Tab/Esc)
- [ ] Icons have aria-hidden="true" (text labels are primary)
- [ ] Loading states announced ("Loading Dashboard...")
```

---

## 8. Performance Optimization Strategy

### 8.1 Performance Budget

**Target Metrics**:
- First Contentful Paint (FCP): < 1.2s
- Largest Contentful Paint (LCP): < 2.5s
- Time to Interactive (TTI): < 3.5s
- Total Blocking Time (TBT): < 300ms
- Cumulative Layout Shift (CLS): < 0.1

**Navigation-Specific Targets**:
- Active state change: < 100ms
- Sidebar animation: 250-300ms
- Navigation click to route start: < 100ms
- Route change complete: < 3s (p80)

### 8.2 Code Splitting Strategy

```typescript
// Dynamic import for heavy components
import dynamic from 'next/dynamic';

// Lazy-load profile dropdown (not critical for initial render)
const ProfileDropdown = dynamic(
  () => import('./ProfileDropdown').then(mod => mod.ProfileDropdown),
  {
    loading: () => <div className="w-8 h-8 animate-pulse bg-gray-200 rounded-full" />,
    ssr: false,
  }
);

// Lazy-load notification dropdown (future enhancement)
const NotificationDropdown = dynamic(
  () => import('./NotificationDropdown'),
  { ssr: false }
);
```

### 8.3 Icon Optimization

**Strategy: SVG Sprite Sheet**

```typescript
// Build-time SVG sprite generation
// src/lib/generate-icon-sprite.ts (run during build)
import { NAVIGATION_ITEMS } from './navigation-config';

export function generateIconSprite() {
  const icons = NAVIGATION_ITEMS.map(item => item.icon);
  // Generate SVG sprite with all icons
  // Output to public/icons-sprite.svg
}

// Usage in components
<svg className="w-6 h-6">
  <use xlinkHref="/icons-sprite.svg#dashboard" />
</svg>
```

**Alternative: Tree-shaken Lucide Icons** (simpler, recommended)
```typescript
// Lucide icons are automatically tree-shaken by bundler
import { LayoutDashboard } from 'lucide-react';
// Only the icons used are included in bundle
```

### 8.4 Animation Performance

```css
/* Use CSS transforms (GPU-accelerated) instead of width */
/* Bad: causes layout recalculation */
.sidebar {
  transition: width 300ms;
}

/* Good: uses GPU compositing */
.sidebar {
  width: 16rem;
  transition: transform 300ms;
}

.sidebar.collapsed {
  transform: translateX(-12rem); /* Shift left, show only 4rem */
}
```

**Implementation**:
```typescript
// Use transform for smoother animation
<aside
  className={cn(
    'fixed left-0 top-0 h-screen w-64 bg-slate-800',
    'transition-transform duration-300 ease-out',
    isSidebarCollapsed && '-translate-x-48' // 64px - 256px = -192px
  )}
>
```

### 8.5 Memoization Strategy

```typescript
// Memoize navigation items (static, never change)
import { useMemo } from 'react';

export function SidebarNav() {
  const pathname = usePathname();

  const navigationItems = useMemo(() => NAVIGATION_ITEMS, []);

  const activeRoute = useMemo(
    () => determineActiveRoute(pathname),
    [pathname]
  );

  return (
    <nav>
      {navigationItems.map((item) => (
        <NavItem
          key={item.id}
          item={item}
          isActive={activeRoute === item.route}
        />
      ))}
    </nav>
  );
}

// Memoize NavItem to prevent unnecessary re-renders
export const NavItem = memo(function NavItem({ item, isActive }: NavItemProps) {
  // Component implementation
});
```

### 8.6 Prefetching Strategy

```typescript
// Prefetch critical routes on app load
export function NavigationProvider({ children }: { children: React.ReactNode }) {
  const router = useRouter();

  useEffect(() => {
    // Prefetch Dashboard and Inventory after initial render
    const prefetchTimer = setTimeout(() => {
      router.prefetch('/');
      router.prefetch('/inventory');
    }, 2000); // Delay to not block initial render

    return () => clearTimeout(prefetchTimer);
  }, [router]);

  return <>{children}</>;
}
```

### 8.7 Bundle Size Optimization

```bash
# Analyze bundle size
npm run build
npx @next/bundle-analyzer

# Expected bundle sizes:
# - Navigation components: ~15KB (gzipped)
# - Lucide icons (6 icons): ~3KB (tree-shaken)
# - Zustand: ~3KB
# - Socket.IO client: ~10KB
# - Total navigation bundle: ~31KB
```

**Optimization Checklist**:
- [ ] Remove unused dependencies
- [ ] Use tree-shakeable icon library (Lucide)
- [ ] Lazy-load non-critical components (dropdowns)
- [ ] Minify and compress assets
- [ ] Enable Brotli compression on server

### 8.8 Render Performance Monitoring

```typescript
// src/components/navigation/NavigationPerformanceMonitor.tsx
'use client';

import { useEffect } from 'react';
import { usePathname } from 'next/navigation';

export function NavigationPerformanceMonitor() {
  const pathname = usePathname();

  useEffect(() => {
    const navigationStart = performance.now();

    const observer = new PerformanceObserver((list) => {
      for (const entry of list.getEntries()) {
        if (entry.entryType === 'navigation') {
          const navigationTime = performance.now() - navigationStart;

          // Log to analytics
          console.log(`Navigation to ${pathname} took ${navigationTime}ms`);

          // Send to analytics service
          if (navigationTime > 3000) {
            console.warn(`Slow navigation detected: ${pathname} (${navigationTime}ms)`);
          }
        }
      }
    });

    observer.observe({ type: 'navigation', buffered: true });

    return () => observer.disconnect();
  }, [pathname]);

  return null;
}
```

---

## 9. Mobile Responsiveness Strategy

### 9.1 Responsive Breakpoints

```typescript
// Tailwind breakpoints (configured in tailwind.config.js)
const breakpoints = {
  sm: '640px',   // Small phones
  md: '768px',   // Tablets (navigation becomes drawer)
  lg: '1024px',  // Desktops (sidebar expanded by default)
  xl: '1280px',  // Large desktops
};
```

**Behavior by Breakpoint**:
| Screen Size | Sidebar | Top Bar | Behavior |
|-------------|---------|---------|----------|
| < 768px (Mobile) | Hidden (drawer) | Hamburger menu | Tap to open overlay |
| 768px - 1024px (Tablet) | Collapsed | Full top bar | Persistent, collapsed |
| ≥ 1024px (Desktop) | Expanded | Full top bar | Persistent, expanded |

### 9.2 Mobile-First Responsive Implementation

```typescript
// Mobile-first CSS classes
<aside
  className={cn(
    // Mobile: hidden by default
    'hidden',

    // Tablet: visible, collapsed
    'md:flex md:flex-col md:w-16',

    // Desktop: visible, expanded
    'lg:w-64',

    // Positioning
    'fixed left-0 top-0 h-screen bg-slate-800',

    // Transitions
    'transition-all duration-300',

    // Conditional states
    !isSidebarCollapsed && 'lg:w-64',
    isSidebarCollapsed && 'lg:w-16'
  )}
>
```

### 9.3 Touch Target Optimization

```typescript
// Ensure minimum 44x44px touch targets on mobile
<button
  className={cn(
    'flex items-center gap-3 rounded-lg transition-colors',
    'px-4 py-3',          // Desktop: comfortable padding
    'md:min-h-[44px]',    // Mobile: ensure 44px height
    'active:bg-white/20', // Mobile: touch feedback
  )}
>
```

### 9.4 Mobile Gesture Support

```typescript
// src/hooks/useSwipeGesture.ts
import { useEffect, useRef } from 'react';

interface SwipeGestureOptions {
  onSwipeLeft?: () => void;
  onSwipeRight?: () => void;
  threshold?: number; // Minimum swipe distance in pixels
}

export function useSwipeGesture(options: SwipeGestureOptions) {
  const touchStart = useRef({ x: 0, y: 0 });
  const threshold = options.threshold || 50;

  useEffect(() => {
    function handleTouchStart(e: TouchEvent) {
      touchStart.current = {
        x: e.touches[0].clientX,
        y: e.touches[0].clientY,
      };
    }

    function handleTouchEnd(e: TouchEvent) {
      const touchEnd = {
        x: e.changedTouches[0].clientX,
        y: e.changedTouches[0].clientY,
      };

      const deltaX = touchEnd.x - touchStart.current.x;
      const deltaY = touchEnd.y - touchStart.current.y;

      // Only trigger if horizontal swipe is greater than vertical
      if (Math.abs(deltaX) > Math.abs(deltaY)) {
        if (deltaX > threshold && options.onSwipeRight) {
          options.onSwipeRight();
        } else if (deltaX < -threshold && options.onSwipeLeft) {
          options.onSwipeLeft();
        }
      }
    }

    document.addEventListener('touchstart', handleTouchStart);
    document.addEventListener('touchend', handleTouchEnd);

    return () => {
      document.removeEventListener('touchstart', handleTouchStart);
      document.removeEventListener('touchend', handleTouchEnd);
    };
  }, [options, threshold]);
}

// Usage in MobileDrawer
export function MobileDrawer() {
  const { setMobileDrawerOpen } = useNavigation();

  useSwipeGesture({
    onSwipeLeft: () => setMobileDrawerOpen(false),
    threshold: 100,
  });

  // Component implementation
}
```

### 9.5 Viewport-Based Layout Adjustment

```typescript
// Adjust layout based on sidebar state
export function NavigationLayout({ children }: { children: React.ReactNode }) {
  const { isSidebarCollapsed } = useNavigationStore();

  return (
    <div className="flex min-h-screen">
      <Sidebar />
      <div
        className={cn(
          'flex-1 transition-all duration-300',
          // Mobile: full width
          'w-full',
          // Tablet/Desktop: account for sidebar
          'md:ml-16',
          !isSidebarCollapsed && 'lg:ml-64'
        )}
      >
        <TopBar />
        <main className="pt-16 p-4 md:p-6 lg:p-8">
          {children}
        </main>
      </div>
    </div>
  );
}
```

### 9.6 Responsive Typography & Spacing

```typescript
// Scale typography and spacing responsively
<h1 className="text-2xl md:text-3xl lg:text-4xl font-semibold">
  Dashboard
</h1>

<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6">
  {/* KPI cards */}
</div>
```

### 9.7 Orientation Change Handling

```typescript
// src/hooks/useOrientation.ts
import { useState, useEffect } from 'react';

export function useOrientation() {
  const [isPortrait, setIsPortrait] = useState(true);

  useEffect(() => {
    function handleOrientationChange() {
      setIsPortrait(window.innerHeight > window.innerWidth);
    }

    handleOrientationChange(); // Initial check
    window.addEventListener('resize', handleOrientationChange);

    return () => window.removeEventListener('resize', handleOrientationChange);
  }, []);

  return isPortrait;
}

// Usage: adjust drawer width based on orientation
export function MobileDrawer() {
  const isPortrait = useOrientation();

  return (
    <aside
      className={cn(
        'fixed top-0 left-0 h-screen bg-slate-800',
        isPortrait ? 'w-80' : 'w-64' // Narrower in landscape
      )}
    >
      {/* Content */}
    </aside>
  );
}
```

---

## 10. Technical Task Breakdown

### TECH-001: Project Setup & Dependencies

**Maps to PRD**: Foundation for all requirements

**Priority**: Critical
**Complexity**: 1

**Technical Description**:
Install and configure all required dependencies for navigation system implementation.

**Tasks**:
1. Install UI component libraries
   ```bash
   npm install lucide-react @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-tooltip
   ```
2. Install state management
   ```bash
   npm install zustand
   ```
3. Install real-time communication
   ```bash
   npm install socket.io-client
   ```
4. Install data fetching
   ```bash
   npm install @tanstack/react-query
   ```
5. Install utilities
   ```bash
   npm install clsx tailwind-merge react-hot-toast
   ```

**Acceptance Criteria**:
- All dependencies installed without conflicts
- TypeScript types available for all libraries
- No peer dependency warnings
- Development server runs without errors

---

### TECH-002: Type Definitions & Configuration

**Maps to PRD**: REQ-NAV-101 (Navigation Configuration)

**Priority**: Critical
**Complexity**: 2

**Technical Description**:
Create TypeScript type definitions and navigation configuration that will be used across all navigation components.

**Implementation**:
```typescript
// src/types/navigation.ts
export interface NavigationItem {
  id: string;
  label: string;
  labelEn: string;
  route: string;
  icon: React.ComponentType<{ className?: string }>;
  shortcut: string;
  shortcutMac: string;
}

export interface UserProfile {
  id: string;
  firstName: string;
  lastName: string;
  email: string;
  role: string;
  profilePhotoUrl: string | null;
}

export interface NotificationSummary {
  unreadCount: number;
  highestPriority: 'critical' | 'high' | 'medium' | 'low';
}
```

**Acceptance Criteria**:
- All types exported and documented
- No TypeScript errors in type files
- Types align with PRD data requirements

---

### TECH-003: Navigation State Management (Zustand Store)

**Maps to PRD**: REQ-NAV-101, REQ-NAV-102, REQ-NAV-103

**Priority**: Critical
**Complexity**: 3

**Technical Description**:
Implement Zustand store for managing navigation state (sidebar collapsed, mobile drawer, notifications, user profile).

**Implementation**:
Create `src/store/navigation.ts` as specified in Section 4.2 (State Slice Design).

**Testing Strategy**:
- Unit tests for store actions
- Verify localStorage persistence
- Test state updates and reactivity

**Acceptance Criteria**:
- Sidebar state persists across page navigations
- Mobile drawer state managed correctly
- Notification count updates trigger re-renders
- localStorage sync works without errors

---

### TECH-004: Navigation Configuration & Utilities

**Maps to PRD**: REQ-NAV-101, REQ-NAV-107

**Priority**: Critical
**Complexity**: 2

**Technical Description**:
Create navigation configuration with all six modules and utility functions for route detection.

**Implementation**:
- Create `src/lib/navigation-config.ts` (Section 5.3)
- Create `src/lib/navigation-utils.ts` (Section 5.2)

**Data Handling**:
- Navigation items are immutable (frozen array)
- Route detection handles both Spanish and English paths
- Icon components imported from Lucide React

**Acceptance Criteria**:
- All six modules configured correctly
- Active route detection works for all routes
- Icons render without errors
- Configuration is tree-shakeable

---

### TECH-005: Sidebar Component (Desktop)

**Maps to PRD**: US-NAV-001, US-NAV-002, REQ-NAV-101

**Priority**: Critical
**Complexity**: 4

**Technical Description**:
Implement desktop sidebar component with logo, navigation items, and collapse button.

**Implementation**:
Create components:
- `src/components/navigation/Sidebar.tsx`
- `src/components/navigation/SidebarLogo.tsx`
- `src/components/navigation/SidebarNav.tsx`
- `src/components/navigation/NavItem.tsx`
- `src/components/navigation/SidebarCollapseButton.tsx`

**Styling**:
- Width: 256px (expanded), 64px (collapsed)
- Background: `#1E3A5F` (dark blue)
- Transition: 300ms ease-out
- Active state: orange background `#F97316`

**Accessibility**:
- Semantic `<nav>` element
- ARIA labels for all interactive elements
- Keyboard focus indicators
- Screen reader announcements

**Acceptance Criteria**:
- All six navigation items visible
- Active state highlights current module
- Click navigates to correct route
- No page reload on same-route click
- Visual feedback within 100ms

---

### TECH-006: Sidebar Collapse/Expand Functionality

**Maps to PRD**: US-NAV-003, REQ-NAV-103

**Priority**: High
**Complexity**: 3

**Technical Description**:
Implement sidebar collapse/expand animation with state persistence and tooltips.

**Implementation**:
- Collapse button toggles Zustand state
- CSS transition animates width change
- Tooltips show on hover in collapsed mode
- State persists to localStorage

**Animation**:
```css
transition: width 300ms cubic-bezier(0.4, 0.0, 0.2, 1);
```

**Tooltip Implementation**:
Use Radix UI Tooltip for accessible hover behavior.

**Acceptance Criteria**:
- Smooth animation (no jank)
- Tooltips appear after 500ms hover
- State persists across sessions
- Content reflows smoothly
- Works in all supported browsers

---

### TECH-007: Active Route Detection Logic

**Maps to PRD**: US-NAV-002, REQ-NAV-102

**Priority**: Critical
**Complexity**: 2

**Technical Description**:
Implement logic to determine active navigation item based on current pathname.

**Implementation**:
- Use Next.js `usePathname()` hook
- Match current path to navigation routes
- Handle sub-routes (e.g., `/inventory/item/123` → Inventario active)
- Recalculate on every route change

**Edge Cases**:
- 404 pages: no active item (or Dashboard fallback)
- Deep links: parent module active
- Browser back/forward: active state follows

**Acceptance Criteria**:
- Active state always accurate
- No flash of incorrect state
- Works with browser navigation
- Handles all PRD edge cases

---

### TECH-008: Top Bar Component

**Maps to PRD**: US-NAV-004, US-NAV-005, REQ-NAV-104, REQ-NAV-105, REQ-NAV-106

**Priority**: Critical
**Complexity**: 3

**Technical Description**:
Implement top navigation bar with app title, notification badge, and user profile menu.

**Implementation**:
Create components:
- `src/components/navigation/TopBar.tsx`
- `src/components/navigation/NotificationBadge.tsx`
- `src/components/navigation/UserProfileMenu.tsx`
- `src/components/navigation/ProfileDropdown.tsx`
- `src/components/navigation/MobileMenuButton.tsx`

**Styling**:
- Height: 64px
- Background: white
- Border bottom: 1px gray-200
- Fixed position, spans full width

**Acceptance Criteria**:
- All elements aligned correctly
- Responsive (adapts to mobile)
- Z-index correct (above content, below modals)

---

### TECH-009: Notification Badge & Real-Time Updates

**Maps to PRD**: US-NAV-004, REQ-NAV-104

**Priority**: Critical
**Complexity**: 4

**Technical Description**:
Implement notification badge with real-time WebSocket updates and polling fallback.

**Implementation**:
- Create `src/lib/websocket/socket-manager.ts` (Section 6.2)
- Create `src/hooks/useNotifications.ts` (Section 6.3)
- Create `src/components/navigation/NotificationBadge.tsx`

**WebSocket Events**:
- `notification:new` → increment count
- `notification:read` → decrement count

**Fallback**:
- Polling every 30 seconds if WebSocket fails

**Acceptance Criteria**:
- Badge shows correct count
- Updates within 10 seconds of server event
- Falls back to polling gracefully
- Count displays "9+" for > 9 notifications
- Badge hidden when count is 0

---

### TECH-010: User Profile Dropdown

**Maps to PRD**: US-NAV-005, REQ-NAV-105

**Priority**: High
**Complexity**: 3

**Technical Description**:
Implement user profile menu with dropdown options (View Profile, Settings, Help, Logout).

**Implementation**:
- Use Radix UI Dropdown Menu for accessibility
- Display user initials or profile photo
- Dropdown menu with 4 options
- Logout confirmation modal

**Dropdown Items**:
1. View Profile → `/profile`
2. Account Settings → `/settings`
3. Help & Documentation → `/help` (new tab)
4. Log Out → confirmation modal → logout API

**Acceptance Criteria**:
- Dropdown opens on click
- Closes on outside click or Escape
- Keyboard navigable (Arrow keys, Enter)
- Logout requires confirmation
- Profile photo/initials display correctly

---

### TECH-011: Mobile Navigation (Drawer/Overlay)

**Maps to PRD**: US-NAV-008, REQ-NAV-108

**Priority**: High
**Complexity**: 4

**Technical Description**:
Implement mobile navigation drawer with hamburger menu, backdrop, and swipe gestures.

**Implementation**:
- Create `src/components/navigation/MobileDrawer.tsx`
- Create `src/components/navigation/HamburgerButton.tsx`
- Create `src/hooks/useSwipeGesture.ts`

**Behavior**:
- Hamburger button opens drawer
- Backdrop click closes drawer
- Swipe left closes drawer
- Navigation click closes drawer and navigates
- Body scroll locked when open

**Styling**:
- Drawer width: 280px (320px max)
- Slide-in animation: 300ms from left
- Backdrop: `rgba(0, 0, 0, 0.5)`
- Z-index: 50 (drawer), 40 (backdrop)

**Acceptance Criteria**:
- Opens/closes smoothly
- Touch targets minimum 44x44px
- Swipe gestures work reliably
- Body scroll locked correctly
- Closes on navigation

---

### TECH-012: Keyboard Shortcuts

**Maps to PRD**: US-NAV-007, REQ-NAV-109

**Priority**: Medium
**Complexity**: 3

**Technical Description**:
Implement global keyboard shortcuts for navigation (Alt+D, Alt+I, etc.).

**Implementation**:
- Create `src/hooks/useKeyboardShortcuts.ts` (Section 7.4)
- Detect platform (Mac vs Windows/Linux)
- Ignore shortcuts when typing in inputs
- Announce navigation to screen readers

**Shortcuts**:
- `Alt+D` / `Cmd+D` → Dashboard
- `Alt+I` / `Cmd+I` → Inventario
- `Alt+C` / `Cmd+C` → Categorías
- `Alt+A` / `Cmd+A` → Alertas
- `Alt+R` / `Cmd+R` → Reportes
- `Alt+S` / `Cmd+S` → Configuración
- `Alt+/` → Keyboard shortcuts help modal

**Acceptance Criteria**:
- All shortcuts work correctly
- Platform-specific modifiers detected
- No conflicts with browser shortcuts
- Shortcuts ignored in text fields
- Help modal accessible via `Alt+/`

---

### TECH-013: Accessibility Implementation (ARIA, Focus, Screen Readers)

**Maps to PRD**: US-NAV-009, NFR-NAV-202

**Priority**: High
**Complexity**: 4

**Technical Description**:
Implement WCAG 2.1 AA accessibility features across all navigation components.

**Tasks**:
1. Add semantic HTML (`<nav>`, `<main>`, `<header>`)
2. Add ARIA labels and roles
3. Implement skip-to-content link
4. Add focus trap for mobile drawer
5. Ensure color contrast meets 4.5:1 ratio
6. Test with screen readers (NVDA, JAWS, VoiceOver)

**Implementation**:
- Create `src/components/navigation/SkipLink.tsx`
- Create `src/hooks/useFocusTrap.ts`
- Add ARIA attributes to all components

**Acceptance Criteria**:
- All navigation functions keyboard-accessible
- Screen reader announces active state
- Focus indicators visible
- No keyboard traps
- Color contrast meets WCAG AA
- Passes automated accessibility audit (axe)

---

### TECH-014: Responsive Layout Integration

**Maps to PRD**: US-NAV-008, NFR-NAV-204

**Priority**: High
**Complexity**: 3

**Technical Description**:
Integrate navigation components into root layout with responsive behavior across breakpoints.

**Implementation**:
- Update `src/app/layout.tsx` to include navigation
- Create `src/components/navigation/NavigationLayout.tsx`
- Implement responsive breakpoints (mobile, tablet, desktop)
- Handle orientation changes

**Breakpoint Behavior**:
- `< 768px`: Mobile drawer
- `768px - 1024px`: Collapsed sidebar
- `≥ 1024px`: Expanded sidebar

**Acceptance Criteria**:
- Correct layout at all breakpoints
- Smooth transitions between breakpoints
- Content reflows correctly
- No layout shift (CLS < 0.1)

---

### TECH-015: API Client for Notifications

**Maps to PRD**: REQ-NAV-104

**Priority**: Critical
**Complexity**: 3

**Technical Description**:
Create API client for fetching notification summary and acknowledging notifications.

**Implementation**:
```typescript
// src/lib/api/notifications.ts
export async function fetchNotificationSummary(): Promise<NotificationSummary> {
  const response = await fetch('/api/notifications/summary', {
    credentials: 'include',
  });

  if (!response.ok) {
    throw new Error('Failed to fetch notifications');
  }

  return response.json();
}

export async function acknowledgeNotification(notificationId: string): Promise<void> {
  const response = await fetch(`/api/notifications/${notificationId}/acknowledge`, {
    method: 'POST',
    credentials: 'include',
  });

  if (!response.ok) {
    throw new Error('Failed to acknowledge notification');
  }
}
```

**Error Handling**:
- Retry on network failure (3 attempts)
- Show user-friendly error message
- Fall back to cached data if available

**Acceptance Criteria**:
- API client handles success and error states
- Credentials sent with requests
- TypeScript types match backend API
- Error handling graceful

---

### TECH-016: API Client for User Profile

**Maps to PRD**: REQ-NAV-105

**Priority**: High
**Complexity**: 2

**Technical Description**:
Create API client for fetching user profile data and handling logout.

**Implementation**:
```typescript
// src/lib/api/user.ts
export async function fetchUserProfile(): Promise<UserProfile> {
  const response = await fetch('/api/user/profile', {
    credentials: 'include',
  });

  if (!response.ok) {
    throw new Error('Failed to fetch user profile');
  }

  return response.json();
}

export async function logout(): Promise<void> {
  const response = await fetch('/api/auth/logout', {
    method: 'POST',
    credentials: 'include',
  });

  if (!response.ok) {
    throw new Error('Failed to logout');
  }

  // Clear local storage
  localStorage.clear();
}
```

**Acceptance Criteria**:
- User profile fetches correctly
- Logout clears session
- Redirects to login after logout

---

### TECH-017: WebSocket Connection Manager

**Maps to PRD**: REQ-NAV-104 (Real-time updates)

**Priority**: Critical
**Complexity**: 4

**Technical Description**:
Implement WebSocket connection manager with auto-reconnection and fallback to polling.

**Implementation**:
Create `src/lib/websocket/socket-manager.ts` as specified in Section 6.2.

**Features**:
- Auto-reconnection with exponential backoff
- Event-based messaging
- Connection state management
- Graceful degradation to polling

**Acceptance Criteria**:
- Connects on user authentication
- Reconnects on disconnect
- Falls back to polling after 5 failed attempts
- Updates notification count in real-time

---

### TECH-018: Performance Optimization (Code Splitting, Prefetching)

**Maps to PRD**: US-NAV-010, NFR-NAV-201

**Priority**: High
**Complexity**: 3

**Technical Description**:
Implement performance optimizations including code splitting, prefetching, and memoization.

**Tasks**:
1. Lazy-load profile dropdown
2. Prefetch critical routes (Dashboard, Inventory)
3. Memoize navigation items
4. Optimize icon loading (tree-shaking)
5. Monitor performance metrics

**Implementation**:
- Use `next/dynamic` for lazy loading
- Use `router.prefetch()` for critical routes
- Use `React.memo()` for NavItem components
- Use `useMemo()` for navigation config

**Acceptance Criteria**:
- Active state change < 100ms
- Navigation bundle < 35KB (gzipped)
- Dashboard prefetched on hover
- No unnecessary re-renders

---

### TECH-019: Internationalization (i18n) Setup

**Maps to PRD**: NFR-NAV-207

**Priority**: Medium
**Complexity**: 3

**Technical Description**:
Set up internationalization framework supporting Spanish (primary) and English (secondary).

**Implementation**:
```bash
npm install next-intl
```

```typescript
// src/lib/i18n.ts
export const locales = ['es', 'en'] as const;
export type Locale = (typeof locales)[number];

export const defaultLocale: Locale = 'es';

export const labels = {
  es: {
    'nav.dashboard': 'Dashboard',
    'nav.inventory': 'Inventario',
    'nav.categories': 'Categorías',
    'nav.alerts': 'Alertas',
    'nav.reports': 'Reportes',
    'nav.configuration': 'Configuración',
  },
  en: {
    'nav.dashboard': 'Dashboard',
    'nav.inventory': 'Inventory',
    'nav.categories': 'Categories',
    'nav.alerts': 'Alerts',
    'nav.reports': 'Reports',
    'nav.configuration': 'Configuration',
  },
};
```

**Acceptance Criteria**:
- Navigation labels display in Spanish by default
- Language can be switched to English
- Browser language detected on first visit
- Language preference persisted

---

### TECH-020: Theme Configuration (PMM Brand Colors)

**Maps to PRD**: REQ-NAV-101 (Visual Specification)

**Priority**: Medium
**Complexity**: 2

**Technical Description**:
Configure Tailwind CSS with PMM brand colors and design tokens.

**Implementation**:
```typescript
// tailwind.config.ts
export default {
  theme: {
    extend: {
      colors: {
        pmm: {
          orange: {
            DEFAULT: '#F97316',
            50: '#FFEDD5',
            500: '#F97316',
            600: '#EA580C',
          },
          slate: {
            800: '#1E293B',
            900: '#0F172A',
          },
        },
      },
    },
  },
};
```

**Update globals.css**:
```css
@theme inline {
  --color-pmm-orange: #F97316;
  --color-pmm-slate: #1E293B;
}
```

**Acceptance Criteria**:
- Brand colors defined in Tailwind config
- Colors match PRD specifications
- Consistent usage across components

---

### TECH-021: Loading States & Error Handling

**Maps to PRD**: REQ-NAV-110

**Priority**: High
**Complexity**: 3

**Technical Description**:
Implement loading states and error handling for navigation interactions.

**Implementation**:
- Loading spinner for slow navigation (> 500ms)
- Error messages for failed navigation
- Retry mechanism for API failures
- Graceful degradation

**Loading States**:
1. 0-500ms: No indicator (optimistic UI)
2. 500ms-3s: Spinner in content area
3. 3s+: Spinner + "Loading..." message
4. 10s+: Error with retry button

**Acceptance Criteria**:
- Loading indicators appear at correct times
- Error messages user-friendly
- Retry button functional
- Navigation remains interactive during load

---

### TECH-022: Unit Tests for Navigation Components

**Maps to PRD**: Testing Strategy

**Priority**: High
**Complexity**: 3

**Technical Description**:
Write unit tests for navigation components using Jest and React Testing Library.

**Test Coverage**:
- NavItem active state rendering
- Sidebar collapse/expand
- Keyboard shortcut handlers
- Active route detection logic
- Notification badge count display

**Implementation**:
```bash
npm install --save-dev @testing-library/react @testing-library/jest-dom vitest
```

```typescript
// src/components/navigation/__tests__/NavItem.test.tsx
import { render, screen } from '@testing-library/react';
import { NavItem } from '../NavItem';

describe('NavItem', () => {
  it('renders active state correctly', () => {
    const item = { id: 'dashboard', label: 'Dashboard', route: '/', ... };
    render(<NavItem item={item} isActive={true} isCollapsed={false} />);

    const link = screen.getByRole('link', { name: /navigate to dashboard/i });
    expect(link).toHaveAttribute('aria-current', 'page');
    expect(link).toHaveClass('bg-orange-500');
  });
});
```

**Acceptance Criteria**:
- Test coverage > 80%
- All critical paths tested
- Tests run in CI/CD pipeline

---

### TECH-023: Integration Tests for Navigation Flows

**Maps to PRD**: Testing Strategy

**Priority**: Medium
**Complexity**: 3

**Technical Description**:
Write integration tests for complete navigation flows using Playwright.

**Test Scenarios**:
1. User navigates from Dashboard to Inventory
2. User collapses sidebar, navigates, sidebar state persists
3. User opens mobile drawer, navigates, drawer closes
4. User receives notification, badge updates
5. Keyboard shortcuts navigate correctly

**Implementation**:
```bash
npm install --save-dev @playwright/test
```

```typescript
// tests/navigation.spec.ts
import { test, expect } from '@playwright/test';

test('sidebar navigation works', async ({ page }) => {
  await page.goto('/');

  // Click Inventory nav item
  await page.click('text=Inventario');

  // Verify navigation
  await expect(page).toHaveURL('/inventory');

  // Verify active state
  const navItem = page.locator('a[aria-current="page"]');
  await expect(navItem).toHaveText('Inventario');
});
```

**Acceptance Criteria**:
- All user stories tested end-to-end
- Tests run on multiple browsers
- Tests run in CI/CD pipeline

---

### TECH-024: Accessibility Audit & Remediation

**Maps to PRD**: NFR-NAV-202

**Priority**: High
**Complexity**: 3

**Technical Description**:
Conduct automated and manual accessibility audits, fix all WCAG 2.1 AA violations.

**Tools**:
- axe DevTools (automated scan)
- WAVE (web accessibility evaluation)
- Lighthouse (accessibility score)
- Manual screen reader testing (NVDA, JAWS, VoiceOver)

**Checklist**:
- [ ] Color contrast meets 4.5:1 ratio
- [ ] All interactive elements keyboard-accessible
- [ ] Focus indicators visible
- [ ] ARIA labels correct
- [ ] Screen reader announces correctly
- [ ] No keyboard traps
- [ ] Touch targets > 44x44px

**Acceptance Criteria**:
- axe audit: 0 violations
- Lighthouse accessibility score: 100
- Manual screen reader test: all functions accessible

---

### TECH-025: Performance Testing & Monitoring

**Maps to PRD**: NFR-NAV-201

**Priority**: Medium
**Complexity**: 3

**Technical Description**:
Set up performance monitoring and conduct performance testing to meet targets.

**Tools**:
- Lighthouse (performance score)
- WebPageTest (real-world performance)
- Vercel Analytics (production monitoring)

**Metrics to Monitor**:
- First Contentful Paint (FCP) < 1.2s
- Largest Contentful Paint (LCP) < 2.5s
- Time to Interactive (TTI) < 3.5s
- Cumulative Layout Shift (CLS) < 0.1
- Navigation click to route start < 100ms

**Implementation**:
```typescript
// src/components/navigation/NavigationPerformanceMonitor.tsx
// (See Section 8.8)
```

**Acceptance Criteria**:
- All performance targets met (p80)
- Performance regression alerts configured
- Performance data visible in dashboard

---

### TECH-026: Documentation (Developer & User)

**Maps to PRD**: Handoff Notes

**Priority**: Medium
**Complexity**: 2

**Technical Description**:
Create comprehensive documentation for developers and end users.

**Developer Documentation**:
- Component API documentation (props, usage examples)
- Architecture diagrams
- Testing guide
- Contribution guide

**User Documentation**:
- Keyboard shortcuts cheat sheet
- Navigation tutorial (video + text)
- FAQ (common navigation issues)
- Accessibility features guide

**Files to Create**:
- `docs/navigation/README.md` (architecture overview)
- `docs/navigation/COMPONENTS.md` (component API)
- `docs/navigation/TESTING.md` (testing guide)
- `docs/navigation/USER_GUIDE.md` (end-user guide)
- `docs/navigation/KEYBOARD_SHORTCUTS.md` (shortcut reference)

**Acceptance Criteria**:
- All components documented
- Code examples included
- Screenshots/diagrams included
- User guide published to help center

---

### TECH-027: CI/CD Pipeline Integration

**Maps to PRD**: Deployment Strategy

**Priority**: Medium
**Complexity**: 2

**Technical Description**:
Integrate navigation tests into CI/CD pipeline for automated quality checks.

**Pipeline Steps**:
1. Lint (ESLint)
2. Type check (TypeScript)
3. Unit tests (Vitest)
4. Integration tests (Playwright)
5. Accessibility audit (axe)
6. Build (Next.js)
7. Performance test (Lighthouse CI)

**Implementation**:
```yaml
# .github/workflows/ci.yml
name: CI
on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check
      - run: npm run test
      - run: npm run test:e2e
      - run: npm run accessibility-audit
      - run: npm run build
```

**Acceptance Criteria**:
- All tests run on every commit
- Failed tests block merge
- Performance regression detected

---

### TECH-028: Rollout Plan & Feature Flags

**Maps to PRD**: Rollout & Change Management Strategy

**Priority**: Medium
**Complexity**: 2

**Technical Description**:
Implement feature flags for phased rollout of navigation system.

**Implementation**:
```typescript
// src/lib/feature-flags.ts
export function isNavigationV2Enabled(userId: string): boolean {
  // Phase 1: Internal testing (user IDs in allowlist)
  const allowlist = ['user-001', 'user-002', ...];
  if (allowlist.includes(userId)) return true;

  // Phase 2: 20% rollout (hash-based)
  const hash = hashUserId(userId);
  if (hash % 100 < 20) return true;

  // Phase 3: Full rollout
  return true;
}
```

**Usage**:
```typescript
export default function Layout({ children }: { children: React.ReactNode }) {
  const { user } = useSession();
  const useNewNav = isNavigationV2Enabled(user?.id);

  if (useNewNav) {
    return <NewNavigationLayout>{children}</NewNavigationLayout>;
  }

  return <LegacyNavigationLayout>{children}</LegacyNavigationLayout>;
}
```

**Acceptance Criteria**:
- Feature flag controls rollout
- Easy rollback mechanism
- Analytics track adoption

---

## 11. Data Architecture

### 11.1 Data Ownership

The Navigation System is a **presentation-only** module with no business data ownership. All data is sourced from other modules:

| Data Type | Owner Module | Navigation Usage |
|-----------|--------------|------------------|
| Notification Count | Alertas Module | Display badge count |
| User Profile | Authentication Module | Display user info |
| Navigation Preferences | Navigation Module | Sidebar collapsed state (UI only) |

### 11.2 Local Storage Schema

```typescript
// localStorage keys
interface NavigationLocalStorage {
  'pmm-navigation': {
    state: {
      isSidebarCollapsed: boolean;
    };
    version: 1;
  };
}

// Example
localStorage.getItem('pmm-navigation')
// => '{"state":{"isSidebarCollapsed":true},"version":1}'
```

**Important**: Only UI preferences stored locally. No sensitive data.

### 11.3 API Response Schemas

**Notification Summary** (`GET /api/notifications/summary`):
```typescript
{
  "unreadCount": 3,
  "highestPriority": "critical",
  "alerts": [
    {
      "id": "ALT-001",
      "type": "out_of_stock",
      "priority": "critical",
      "message": "Item XYZ is out of stock",
      "timestamp": "2026-02-05T10:30:00Z"
    }
  ]
}
```

**User Profile** (`GET /api/user/profile`):
```typescript
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

### 11.4 WebSocket Event Schemas

**Notification Events**:
```typescript
// Server -> Client
socket.emit('notification:new', {
  unreadCount: 4,
  notification: {
    id: "ALT-002",
    type: "low_stock",
    priority: "high",
    message: "Item ABC is below reorder point",
    timestamp: "2026-02-05T11:00:00Z"
  }
});

socket.emit('notification:read', {
  unreadCount: 3,
  notificationId: "ALT-001"
});
```

### 11.5 Data Caching Strategy

**React Query Cache Configuration**:
```typescript
// src/lib/query-client.ts
import { QueryClient } from '@tanstack/react-query';

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000,      // 5 minutes
      cacheTime: 10 * 60 * 1000,     // 10 minutes
      refetchOnWindowFocus: true,     // Refresh on tab focus
      refetchOnReconnect: true,       // Refresh on reconnect
      retry: 3,                       // Retry failed requests
    },
  },
});
```

**Cache Keys**:
```typescript
const queryKeys = {
  notifications: {
    summary: ['notifications', 'summary'],
    list: (page: number) => ['notifications', 'list', page],
  },
  user: {
    profile: ['user', 'profile'],
  },
};
```

---

## 12. API & Integration Design

### 12.1 Backend API Endpoints (Expected)

**Notification API**:
```
GET    /api/notifications/summary
POST   /api/notifications/{id}/acknowledge
GET    /api/notifications
```

**User API**:
```
GET    /api/user/profile
POST   /api/auth/logout
```

**WebSocket**:
```
ws://localhost:3001 (development)
wss://pmm.example.com/ws (production)
```

### 12.2 API Client Architecture

```typescript
// src/lib/api/client.ts
const API_BASE_URL = process.env.NEXT_PUBLIC_API_URL || 'http://localhost:3000';

async function apiClient<T>(
  endpoint: string,
  options?: RequestInit
): Promise<T> {
  const response = await fetch(`${API_BASE_URL}${endpoint}`, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      ...options?.headers,
    },
    credentials: 'include', // Send cookies
  });

  if (!response.ok) {
    const error = await response.json().catch(() => ({}));
    throw new Error(error.message || 'API request failed');
  }

  return response.json();
}

export { apiClient };
```

### 12.3 Error Handling Strategy

**API Errors**:
```typescript
// src/lib/api/notifications.ts
import { toast } from 'react-hot-toast';

export async function fetchNotificationSummary(): Promise<NotificationSummary> {
  try {
    return await apiClient<NotificationSummary>('/api/notifications/summary');
  } catch (error) {
    console.error('Failed to fetch notifications:', error);
    toast.error('No se pudieron cargar las notificaciones');

    // Return cached data if available
    const cached = queryClient.getQueryData(['notifications', 'summary']);
    if (cached) return cached as NotificationSummary;

    // Fallback to zero state
    return { unreadCount: 0, highestPriority: 'low' };
  }
}
```

### 12.4 Authentication Integration

**Session Management**:
```typescript
// src/hooks/useSession.ts
export function useSession() {
  const { data: user, isLoading } = useQuery({
    queryKey: ['user', 'profile'],
    queryFn: fetchUserProfile,
    staleTime: Infinity, // Don't refetch unless explicitly invalidated
  });

  return {
    user,
    isLoading,
    isAuthenticated: !!user,
    sessionToken: getCookie('session_token'),
  };
}
```

**Logout Flow**:
```typescript
export async function handleLogout() {
  // Show confirmation
  const confirmed = await confirm('¿Está seguro de que desea cerrar sesión?');
  if (!confirmed) return;

  // Call logout API
  await logout();

  // Clear all cached data
  queryClient.clear();

  // Redirect to login
  router.push('/login');
}
```

---

## 13. Testing Strategy

### 13.1 Testing Pyramid

```
         /\
        /  \      E2E Tests (10%)
       /____\     - Full user flows
      /      \    - Critical paths only
     /________\
    /          \  Integration Tests (30%)
   /____________\ - Component interactions
  /              \
 /________________\ Unit Tests (60%)
                    - Individual functions
                    - Component logic
```

### 13.2 Unit Testing

**Tools**: Vitest + React Testing Library

**Test Files**:
```
src/components/navigation/__tests__/
├── NavItem.test.tsx
├── Sidebar.test.tsx
├── NotificationBadge.test.tsx
├── UserProfileMenu.test.tsx
└── MobileDrawer.test.tsx

src/lib/__tests__/
├── navigation-utils.test.ts
├── navigation-config.test.ts
└── websocket/socket-manager.test.ts

src/hooks/__tests__/
├── useNotifications.test.ts
├── useKeyboardShortcuts.test.ts
└── useActiveRoute.test.ts
```

**Example Unit Test**:
```typescript
// src/lib/__tests__/navigation-utils.test.ts
import { determineActiveRoute } from '../navigation-utils';

describe('determineActiveRoute', () => {
  it('returns "/" for root path', () => {
    expect(determineActiveRoute('/')).toBe('/');
  });

  it('returns "/inventory" for inventory paths', () => {
    expect(determineActiveRoute('/inventory')).toBe('/inventory');
    expect(determineActiveRoute('/inventory/item/123')).toBe('/inventory');
  });

  it('handles Spanish routes', () => {
    expect(determineActiveRoute('/inventario')).toBe('/inventory');
  });

  it('returns "/" for unknown paths', () => {
    expect(determineActiveRoute('/unknown')).toBe('/');
  });
});
```

### 13.3 Integration Testing

**Tools**: Playwright

**Test Scenarios**:
```typescript
// tests/navigation/sidebar.spec.ts
test.describe('Sidebar Navigation', () => {
  test('navigates between modules', async ({ page }) => {
    await page.goto('/');

    // Verify Dashboard is active
    await expect(page.locator('[aria-current="page"]')).toHaveText('Dashboard');

    // Click Inventory
    await page.click('text=Inventario');

    // Verify navigation
    await expect(page).toHaveURL('/inventory');
    await expect(page.locator('[aria-current="page"]')).toHaveText('Inventario');
  });

  test('persists collapsed state', async ({ page }) => {
    await page.goto('/');

    // Collapse sidebar
    await page.click('[aria-label*="Collapse"]');

    // Navigate to another page
    await page.click('text=Inventario');

    // Verify sidebar still collapsed
    const sidebar = page.locator('aside[aria-label="Main navigation"]');
    await expect(sidebar).toHaveClass(/w-16/);
  });

  test('mobile drawer opens and closes', async ({ page }) => {
    await page.setViewportSize({ width: 375, height: 667 });
    await page.goto('/');

    // Open drawer
    await page.click('[aria-label*="menu"]');

    // Verify drawer visible
    await expect(page.locator('[role="dialog"]')).toBeVisible();

    // Click navigation item
    await page.click('text=Inventario');

    // Verify drawer closed and navigated
    await expect(page.locator('[role="dialog"]')).toBeHidden();
    await expect(page).toHaveURL('/inventory');
  });
});
```

### 13.4 Accessibility Testing

**Automated**:
```typescript
// tests/accessibility/navigation.spec.ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('navigation has no accessibility violations', async ({ page }) => {
  await page.goto('/');

  const accessibilityScanResults = await new AxeBuilder({ page }).analyze();

  expect(accessibilityScanResults.violations).toEqual([]);
});
```

**Manual Testing Checklist**:
- [ ] Keyboard-only navigation (Tab, Enter, Escape)
- [ ] Screen reader announces correctly (NVDA, JAWS, VoiceOver)
- [ ] Focus indicators visible
- [ ] Color contrast meets WCAG AA
- [ ] Touch targets > 44x44px on mobile

### 13.5 Performance Testing

**Lighthouse CI**:
```yaml
# .github/workflows/lighthouse.yml
- name: Run Lighthouse CI
  uses: treosh/lighthouse-ci-action@v9
  with:
    urls: |
      http://localhost:3000/
      http://localhost:3000/inventory
    budgetPath: ./lighthouse-budget.json
    uploadArtifacts: true
```

**Performance Budget** (`lighthouse-budget.json`):
```json
{
  "timings": [
    { "metric": "interactive", "budget": 3500 },
    { "metric": "first-contentful-paint", "budget": 1200 },
    { "metric": "largest-contentful-paint", "budget": 2500 }
  ],
  "resourceSizes": [
    { "resourceType": "script", "budget": 250 },
    { "resourceType": "stylesheet", "budget": 50 },
    { "resourceType": "total", "budget": 500 }
  ]
}
```

### 13.6 Visual Regression Testing

**Tools**: Playwright + Percy

```typescript
// tests/visual/navigation.spec.ts
test('sidebar visual regression', async ({ page }) => {
  await page.goto('/');

  // Take screenshot
  await percySnapshot(page, 'Sidebar - Expanded');

  // Collapse sidebar
  await page.click('[aria-label*="Collapse"]');
  await percySnapshot(page, 'Sidebar - Collapsed');
});
```

---

## 14. Risk Assessment & Mitigation

### 14.1 Technical Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| **WebSocket connection failures in production** | Medium | High | Implement robust polling fallback, retry logic with exponential backoff, monitor connection health |
| **Performance regression on low-end devices** | Medium | Medium | Performance budget in CI, test on real devices, reduce animations on low-end hardware |
| **Accessibility violations** | Low | High | Automated axe audits in CI, manual screen reader testing, accessibility champion review |
| **State sync issues across browser tabs** | Low | Medium | Use BroadcastChannel API for cross-tab sync, test multi-tab scenarios |
| **localStorage quota exceeded** | Very Low | Low | Only store minimal UI state, implement quota check, fallback to session storage |
| **Keyboard shortcut conflicts with browser/OS** | Medium | Low | Use Alt/Cmd modifiers, provide alternative shortcuts, document conflicts |

### 14.2 Business Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| **User confusion during rollout** | Medium | Medium | Phased rollout, in-app onboarding, video tutorial, clear communication |
| **Increased support tickets** | Low | Medium | Comprehensive user documentation, FAQ, support team training |
| **User resistance to change** | Low | Low | Highlight benefits, provide opt-in period, collect feedback |

### 14.3 Integration Risks

| Risk | Probability | Impact | Mitigation Strategy |
|------|-------------|--------|---------------------|
| **Backend API not ready** | Low | High | Define API contract early, use mock API for development, parallel development |
| **WebSocket server delays** | Medium | Medium | Start with polling implementation, add WebSocket later, feature flag |
| **Authentication middleware conflicts** | Low | Medium | Coordinate with auth team, test session handling thoroughly |

### 14.4 Data Integrity Risks

**Not Applicable**: Navigation is presentation-only with no business data mutations.

### 14.5 Mitigation Actions

**Pre-Launch**:
1. Comprehensive testing (unit, integration, E2E)
2. Accessibility audit by external auditor
3. Performance testing on real devices
4. Security review (XSS, CSRF)
5. Load testing for WebSocket connections

**Post-Launch**:
1. Monitor error rates (Sentry)
2. Monitor performance (Vercel Analytics)
3. Track user feedback (surveys)
4. A/B test navigation changes
5. Quick rollback mechanism (feature flags)

---

## 15. Implementation Phasing

### Phase 1: Foundation (Week 1-2)

**Goal**: Set up project infrastructure and core navigation structure

**Tasks**:
- TECH-001: Project Setup & Dependencies
- TECH-002: Type Definitions & Configuration
- TECH-003: Navigation State Management
- TECH-004: Navigation Configuration & Utilities
- TECH-020: Theme Configuration

**Deliverables**:
- All dependencies installed
- TypeScript types defined
- Zustand store implemented
- Navigation config created
- Tailwind theme configured

**Success Criteria**:
- Development server runs without errors
- TypeScript compiles successfully
- Navigation config accessible

---

### Phase 2: Core Components (Week 3-4)

**Goal**: Build primary navigation components (sidebar, top bar)

**Tasks**:
- TECH-005: Sidebar Component (Desktop)
- TECH-006: Sidebar Collapse/Expand Functionality
- TECH-007: Active Route Detection Logic
- TECH-008: Top Bar Component
- TECH-014: Responsive Layout Integration

**Deliverables**:
- Functional desktop sidebar
- Collapsible sidebar with animations
- Top bar with app title
- Active state highlighting
- Responsive layout wrapper

**Success Criteria**:
- Sidebar displays all six modules
- Active state updates on navigation
- Collapse/expand works smoothly
- Layout integrates with Next.js App Router

---

### Phase 3: Notifications & User Profile (Week 5)

**Goal**: Implement notification badge and user profile menu

**Tasks**:
- TECH-009: Notification Badge & Real-Time Updates
- TECH-010: User Profile Dropdown
- TECH-015: API Client for Notifications
- TECH-016: API Client for User Profile

**Deliverables**:
- Notification badge component
- User profile dropdown
- API clients for data fetching
- WebSocket connection (or polling fallback)

**Success Criteria**:
- Badge displays notification count
- Profile dropdown shows user info
- Logout flow works correctly
- Real-time updates functional (or polling)

---

### Phase 4: Mobile & Accessibility (Week 6)

**Goal**: Add mobile support and accessibility features

**Tasks**:
- TECH-011: Mobile Navigation (Drawer/Overlay)
- TECH-012: Keyboard Shortcuts
- TECH-013: Accessibility Implementation
- TECH-019: Internationalization Setup

**Deliverables**:
- Mobile drawer navigation
- Keyboard shortcuts functional
- ARIA labels and roles added
- Spanish/English language support

**Success Criteria**:
- Mobile drawer works on touch devices
- All shortcuts functional
- Passes axe accessibility audit
- Spanish labels display correctly

---

### Phase 5: Performance & Testing (Week 7-8)

**Goal**: Optimize performance and complete test coverage

**Tasks**:
- TECH-018: Performance Optimization
- TECH-022: Unit Tests
- TECH-023: Integration Tests
- TECH-024: Accessibility Audit
- TECH-025: Performance Testing

**Deliverables**:
- Optimized bundle size (< 35KB)
- 80%+ test coverage
- E2E tests for critical flows
- Accessibility audit report
- Performance benchmark results

**Success Criteria**:
- All performance targets met
- All tests passing
- Zero accessibility violations
- Lighthouse score > 95

---

### Phase 6: Documentation & Deployment (Week 9)

**Goal**: Complete documentation and prepare for rollout

**Tasks**:
- TECH-026: Documentation
- TECH-027: CI/CD Pipeline Integration
- TECH-028: Rollout Plan & Feature Flags
- TECH-017: WebSocket Connection Manager (if not already done)
- TECH-021: Loading States & Error Handling

**Deliverables**:
- Developer documentation
- User guide and video tutorial
- CI/CD pipeline configured
- Feature flags implemented
- Production-ready build

**Success Criteria**:
- All documentation complete
- CI/CD pipeline green
- Feature flags control rollout
- Production deployment successful

---

### Phase 7: Rollout & Monitoring (Week 10-12)

**Goal**: Gradual rollout and post-launch monitoring

**Rollout Schedule**:
- **Week 10**: Internal testing (10 users)
- **Week 11**: Pilot rollout (20% of users)
- **Week 12**: Full rollout (100% of users)

**Monitoring**:
- Error rates (Sentry)
- Performance metrics (Vercel Analytics)
- User feedback (surveys)
- Support tickets

**Success Criteria**:
- Error rate < 1%
- User satisfaction > 4.2/5
- Navigation-related support tickets < baseline

---

### Critical Path

**Must-Complete-First Tasks** (blockers):
1. TECH-001 → TECH-002 → TECH-003 (Foundation)
2. TECH-004 → TECH-005 → TECH-007 (Sidebar)
3. TECH-015 → TECH-009 (Notifications)
4. TECH-016 → TECH-010 (User Profile)

**Parallel Tracks**:
- Mobile (TECH-011) can be developed in parallel with desktop
- Accessibility (TECH-013) can be integrated continuously
- Testing (TECH-022, TECH-023) can be written alongside components

---

## Quality Gate

Before moving to production, the following criteria must be met:

**Functionality**:
- [ ] All PRD user stories implemented
- [ ] All acceptance criteria met
- [ ] No critical bugs

**Performance**:
- [ ] Active state change < 100ms
- [ ] Navigation to page < 3s (p80)
- [ ] Lighthouse performance score > 90

**Accessibility**:
- [ ] WCAG 2.1 AA compliant
- [ ] axe audit: 0 violations
- [ ] Manual screen reader test passed

**Testing**:
- [ ] Unit test coverage > 80%
- [ ] All E2E tests passing
- [ ] Visual regression tests passing

**Documentation**:
- [ ] Developer docs complete
- [ ] User guide published
- [ ] API contracts documented

**Security**:
- [ ] No XSS vulnerabilities
- [ ] Session handling secure
- [ ] No sensitive data in localStorage

**Deployment**:
- [ ] CI/CD pipeline green
- [ ] Feature flags configured
- [ ] Rollback plan tested

---

## Handoff Notes

### For Development Team

**Priority Clarifications**:
- Focus on **Desktop Sidebar** first (Phase 2) - highest user impact
- **WebSocket** can be replaced with polling initially if backend delays
- **Mobile drawer** is lower priority than desktop (warehouse staff are minority)

**Technical Decisions Left to Team**:
- Exact animation easing curve (300ms is target, adjust for smoothness)
- Toast notification library (react-hot-toast suggested, alternatives OK)
- Icon library final choice (Lucide recommended, Heroicons also acceptable)

### Open Questions for Product Review

1. **Notification Dropdown (Future)**: Should clicking badge navigate to `/alerts` or open inline dropdown?
   - **Recommendation**: Navigate to `/alerts` for MVP (simpler), add dropdown in v1.1

2. **Logo in Collapsed Mode**: Show icon only or hide completely?
   - **Recommendation**: Show icon only (maintains branding)

3. **Keyboard Shortcut Conflicts**: What if user has browser extension using same shortcuts?
   - **Recommendation**: Provide alternative number-based shortcuts (`Ctrl+1-6`)

4. **Dark Mode**: Implement now or defer to v2.0?
   - **Recommendation**: Defer to v2.0 (not in PRD, adds complexity)

---

**End of Technical Architecture & Implementation Plan**
