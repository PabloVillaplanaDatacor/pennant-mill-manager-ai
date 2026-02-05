# Senior Software Architect - Persistent Memory

## Project Context

**Pennant Mill Manager (PMM)**: Manufacturing operations system built with Next.js 16, React 19, and Tailwind CSS 4.

## Key Architectural Patterns

### Component Architecture
- Use component-based layered architecture for UI features (Presentation → Application → Domain → Integration)
- Separate mobile and desktop components when interaction patterns differ significantly
- Server Components by default, Client Components only for interactivity
- Leverage Next.js 16 App Router for SSR and automatic code splitting

### State Management
- **Zustand** preferred over Redux for UI state (smaller bundle, simpler API)
- Use React Context to provide Zustand store to component tree
- Persist only UI preferences to localStorage (never sensitive data)
- Derive active state from URL (Next.js usePathname) rather than managing separately

### Real-Time Features
- **Socket.IO Client** preferred over native WebSocket (auto-reconnection, polling fallback)
- Always implement polling fallback for WebSocket (30-second interval)
- Use React Query for API data fetching with smart caching (5-min stale time)

### Accessibility First
- Build with Radix UI primitives for inherent WCAG 2.1 AA compliance
- Always include skip-to-content links
- Implement keyboard shortcuts with platform detection (Alt/Cmd modifiers)
- Test with screen readers (NVDA, JAWS, VoiceOver) before launch

### Performance Optimization
- Target bundle size < 35KB for feature modules
- Use CSS transforms for animations (GPU-accelerated) instead of width/height
- Lazy-load non-critical components (dropdowns, modals)
- Prefetch critical routes on hover
- Memoize static configuration data

## PMM-Specific Conventions

### Manufacturing System Priorities
1. **Data Integrity**: Always primary concern (though navigation is presentation-only)
2. **Audit Trail**: Log all significant user actions
3. **Historical Immutability**: Avoid destructive operations
4. **Multi-Site Scale**: Design for enterprise from day one

### Navigation Module Specifics
- Fixed navigation order (Dashboard → Inventory → Categories → Alerts → Reports → Configuration)
- Spanish primary language, English secondary
- Orange (#F97316) for active states, dark blue (#1E3A5F) for sidebar
- Mobile breakpoint at 768px (drawer mode)
- Desktop sidebar defaults: collapsed < 1024px, expanded >= 1024px

## Technology Stack Decisions

### Preferred Libraries
- **Icons**: Lucide React (tree-shakeable, consistent 24px sizing)
- **UI Primitives**: Radix UI (accessible, headless, composable)
- **State**: Zustand (lightweight, TypeScript-first)
- **Data Fetching**: TanStack Query (caching, optimistic updates)
- **WebSocket**: Socket.IO Client (robust, fallback support)
- **Testing**: Vitest (unit), Playwright (E2E), axe-core (accessibility)

### Avoid
- Redux (too heavy for this use case)
- Native WebSocket without fallback (reliability issues)
- Font Awesome (large bundle, not tree-shakeable)
- CSS-in-JS libraries (Tailwind already configured)

## Task Breakdown Strategy

### Complexity Levels
- **1**: Configuration/setup (< 2 hours)
- **2**: Simple component (2-4 hours)
- **3**: Complex component or integration (1 day)
- **4**: Multi-component feature (2-3 days)
- **5**: Architecture-level work (1 week)

### Priority Mapping
- **Critical**: Blocks all other work or user-facing core feature
- **High**: Important for UX but not blocking
- **Medium**: Nice-to-have, can be deferred
- **Low**: Future enhancement

## Common Pitfalls

### State Management
- **Mistake**: Storing derived state (active route) in Zustand
- **Correct**: Derive from usePathname() hook, recalculate on route change

### Accessibility
- **Mistake**: Relying solely on hover for tooltips
- **Correct**: Use Radix Tooltip with keyboard trigger support

### Performance
- **Mistake**: Animating width/height (causes layout recalculation)
- **Correct**: Use transform: translateX() for GPU acceleration

### Mobile
- **Mistake**: Same component for desktop and mobile (conflicting logic)
- **Correct**: Separate Sidebar (desktop) and MobileDrawer components

## Architecture Document Structure

### Essential Sections (for PMM)
1. Architecture Overview (layering, technology stack, data flow)
2. Current Codebase Analysis (gaps, compatibility)
3. Component Architecture (hierarchy, specifications)
4. State Management Strategy (Zustand store design)
5. Technical Task Breakdown (TECH-XXX format, maps to PRD REQ-XXX)
6. Data Architecture (ownership, schemas, caching)
7. API & Integration Design (endpoints, error handling)
8. Testing Strategy (unit, integration, E2E, accessibility)
9. Risk Assessment & Mitigation (technical, business, integration)
10. Implementation Phasing (week-by-week, critical path)

### Task Format Template
```
TECH-XXX: Task Name
Maps to PRD: REQ-XXX
Priority: Critical/High/Medium/Low
Complexity: 1-5

Technical Description: What needs to be built
Implementation: Code examples, architecture decisions
Testing Strategy: How to verify
Acceptance Criteria: Definition of done
```

## Lessons Learned

### Navigation System Architecture
- **WebSocket Fallback is Critical**: Production environments may block WebSocket; polling must work standalone
- **Server-Side Hydration Prevents Flash**: Render with default state, hydrate client-side to avoid incorrect initial render
- **Feature Flags Enable Safe Rollout**: New navigation can coexist with legacy using feature flags
- **Accessibility Cannot Be Retrofitted**: Build with semantic HTML and ARIA from the start

### Next.js 16 App Router
- Use `usePathname()` for client-side route detection (not `useRouter().pathname`)
- Server Components are default; mark interactive components with `'use client'`
- Metadata API in layout.tsx for dynamic page titles
- Automatic code splitting per route (no manual lazy loading needed)

### Tailwind CSS 4
- New `@theme` directive for CSS custom properties
- Container queries available for responsive components
- Dark mode via `prefers-color-scheme` or class strategy

## Reference Files

### Key Project Files
- `src/app/layout.tsx` - Root layout, navigation integration point
- `src/app/globals.css` - Tailwind imports, theme variables
- `package.json` - Dependency versions
- `tsconfig.json` - Path aliases (@/* → src/*)

### Documentation Templates
- `software-factory/<feature>/product-requirements.md` - PRD from Product Manager
- `software-factory/<feature>/technical-plan.md` - This architect's output
- `software-factory/<feature>/technical-summary.json` - Machine-readable summary

## Team Collaboration

### Handoff to Developers
- Provide complete component specifications with TypeScript interfaces
- Include implementation examples (not just descriptions)
- Call out architectural constraints (e.g., "must use Zustand, not Context alone")
- List open questions requiring product/backend coordination

### Coordination Points
- **Backend Team**: API contracts, WebSocket server setup, authentication
- **Product Manager**: Open questions on UX behavior (e.g., notification dropdown vs navigation)
- **QA Team**: Test scenarios, accessibility requirements, performance targets

---

*Last Updated: 2026-02-05*
*Navigation System Architecture v1.0*
