# 🎨 Frontend Developer — Skills Guide

Frontend developers own everything the user sees and touches. This guide covers UI frameworks, state management, design systems, animations, performance, accessibility, and testing — with practical `@skill-name` invocations for each.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Frameworks | `@react-best-practices`, `@nextjs-app-router-patterns`, `@angular`, `@sveltekit` |
| State | `@react-state-management`, `@zustand-store-ts`, `@tanstack-query-expert` |
| Design Systems | `@design-system`, `@shadcn`, `@tailwind-design-system`, `@radix-ui-design-system` |
| UI Styles | `@design-it/*` (40+ styles), `@high-end-visual-design`, `@magic-ui-generator` |
| Animations | `@animejs-animation`, `@ui-motion`, `@threejs-skills`, `@magic-animator` |
| Performance | `@web-performance-optimization`, `@frontend-lighthouse`, `@react-component-performance` |
| Accessibility | `@accessibility-compliance-accessibility-audit`, `@ui-a11y`, `@wcag-audit-patterns` |
| Testing | `@cypress-skill`, `@playwright-skill`, `@vitest-skill`, `@jest-skill` |
| Tooling | `@nx-workspace-patterns`, `@turborepo-caching`, `@bun-development` |

---

## ⚛️ Core Frameworks

### `@react-best-practices`
Use on any React component review, refactor, or architecture decision.
```
@react-best-practices Review this component:
[paste component]
Check for: unnecessary re-renders, stale closures, missing deps in useEffect,
prop drilling that should be context, missing error boundaries.
```

### `@nextjs-app-router-patterns`
Next.js App Router patterns — server components, streaming, parallel routes.
```
@nextjs-app-router-patterns Implement a products page with:
- Server component for initial data fetch
- Parallel route for product detail modal
- Intercepting route so modal shows on click but page loads on direct URL
- Suspense boundaries for graceful loading
```

### `@react-patterns`
Compound components, render props, higher-order components, slots.
```
@react-patterns Implement a flexible DataTable component using:
- Compound component pattern (Table.Header, Table.Body, Table.Row)
- Render props for custom cell rendering
- Context for shared sorting/filtering state
```

### `@angular` / `@angular-best-practices` / `@angular-state-management`
For Angular: components, services, RxJS, NgRx, signals.
```
@angular-best-practices Audit this Angular component for:
- Change detection strategy (OnPush opportunities)
- RxJS subscription memory leaks
- Template performance issues
- Missing accessibility attributes
```

### `@sveltekit` / `@astro`
For Svelte and Astro-based projects.
```
@sveltekit Implement a form with:
- Server actions for submission
- Progressive enhancement (works without JS)
- Zod validation on both client and server
- Loading states and error display
```

---

## 🔄 State Management

### `@react-state-management`
Helps decide between local state, context, Zustand, Jotai, or server state.
```
@react-state-management Our app has:
- User auth state (global)
- Shopping cart (global, persisted)
- Filter/sort state for product list (local to page)
- Server data (server state)
Recommend the right tools for each. Show implementation patterns.
```

### `@zustand-store-ts`
Zustand store design with TypeScript, slices, persistence, devtools.
```
@zustand-store-ts Create a Zustand store for a shopping cart:
- addItem, removeItem, updateQuantity, clearCart
- Compute derived values: totalItems, totalPrice, hasItem(id)
- Persist to localStorage
- TypeScript typed throughout
```

### `@tanstack-query-expert`
TanStack Query for server state, caching, mutations.
```
@tanstack-query-expert Set up TanStack Query for our events app:
- Query: paginated events list with filters (staleTime: 5min)
- Mutation: register for event with optimistic update
- Mutation: cancel registration with rollback on error
- Prefetch on hover for event cards
```

---

## 🎨 Design Systems & UI

### `@design-system`
Creating or implementing a design system.
```
@design-system Create a design system for our events platform:
- Color tokens: primary, secondary, semantic (error, success, warning)
- Typography scale: heading, body, caption
- Spacing scale: 4px base
- Component primitives: Button, Input, Card, Badge, Modal
- Dark mode support
Output as CSS custom properties + Tailwind config.
```

### `@shadcn`
Working with shadcn/ui components and customizing them.
```
@shadcn Add a custom EventCard based on shadcn's Card primitive:
- Shows: event image, title, date, location, spots remaining
- Badge for category
- Skeleton loading state
- Hover animation
Extend using cva for variants.
```

### `@tailwind-design-system` / `@tailwind-patterns`
Tailwind CSS architecture, custom plugins, design tokens.

---

## 🎭 Visual Design Styles (`@design-it/*`)

The `design-it` family has **40+ visual style sub-skills**:

| Style | Invoke | Best For |
|---|---|---|
| Glassmorphism | `@design-it/glassmorphism` | Modern SaaS dashboards |
| Dark Mode | `@design-it/dark-mode` | Developer tools |
| Neo Brutalism | `@design-it/neo-brutalism` | Bold, standout marketing |
| Neumorphism | `@design-it/neumorphism` | iOS-like premium feel |
| Minimalism | `@design-it/minimalism` | Clean, content-heavy apps |
| Material Design | `@design-it/material-design` | Android / Google ecosystem |
| Cyberpunk | `@design-it/cyberpunk-ui` | Gaming, tech products |
| Synthwave | `@design-it/synthwave` | Creative, entertainment |
| Retro | `@design-it/retro-design` | Nostalgia-driven brands |
| Swiss Design | `@design-it/swiss-design` | Typography-first, editorial |
| Vaporwave | `@design-it/vaporwave` | Art / aesthetic projects |
| Y2K | `@design-it/y2k-design` | Trendy fashion brands |
| Holographic | `@design-it/holographic-ui` | Futuristic / sci-fi |
| Sci-Fi | `@design-it/sci-fi-interface` | Data dashboards |

```
@design-it/glassmorphism Design a login page for our events platform.
Background: blurred campus photo. Card: frosted glass with subtle border.
Blue/purple gradient accents. Subtle backdrop filter animations.
Output: complete HTML + CSS.
```

---

## ✨ Animations & Motion

### `@animejs-animation`
```
@animejs-animation Staggered entrance animation for event cards:
- Fade-in from bottom (100ms delay between cards)
- Scale 0.95 → 1 on enter
- Respect prefers-reduced-motion
```

### `@ui-motion`
```
@ui-motion Add motion design to our events platform:
- Page transitions (slide in from right)
- Modal open/close (scale + opacity)
- Button press feedback
- Skeleton pulse
Following the 12 principles of animation.
```

### `@threejs-skills` / `@3d-web-experience`
```
@threejs-skills Create an interactive 3D campus map:
- Basic Three.js scene
- GLB model loading
- Click buildings to see events
- Orbit controls with mobile touch
```

---

## ⚡ Performance

### `@web-performance-optimization`
```
@web-performance-optimization Audit our Next.js platform:
- Core Web Vitals targets: LCP < 2.5s, FID < 100ms, CLS < 0.1
- Image optimization strategy
- Font loading optimization
- Bundle analysis + code splitting
Give me a prioritized action list.
```

### `@react-component-performance`
```
@react-component-performance Our events list renders 200+ items and is slow.
Analyze this EventList component. Apply: React.memo, useMemo, virtualization.
[paste component]
```

### `@frontend-lighthouse`
```
@frontend-lighthouse Our Lighthouse score is 67 on mobile.
Top issues: render-blocking resources, unused CSS, no lazy loading.
Give me specific code changes to exceed 90.
```

---

## ♿ Accessibility

### `@accessibility-compliance-accessibility-audit`
```
@accessibility-compliance-accessibility-audit Audit EventCard component:
Check: color contrast (WCAG AA), keyboard navigation, ARIA labels,
focus indicators, screen reader order, alt text.
```

### `@ui-a11y`
```
@ui-a11y Make this modal fully accessible:
- Focus trap when open, return focus on close
- ESC key closes
- ARIA: role, aria-modal, aria-labelledby, aria-describedby
- Screen reader announces opening
```

---

## 🧪 Testing

### `@vitest-skill` / `@jest-skill`
```
@vitest-skill Write tests for EventCard component:
- Renders with all props
- Shows "Sold Out" badge when capacity reached
- Click handler fires with correct event ID
- Skeleton renders during loading
```

### `@playwright-skill` / `@cypress-skill`
```
@playwright-skill E2E test for registration flow:
1. Visit /events
2. Click event card
3. Click "Register Now"
4. Fill form + Stripe test payment
5. Assert QR ticket visible
```

---

## 🔗 Complete Frontend Prompt Chain

```
1️⃣  @design-thinking
    "Define UX goals for the events page"

2️⃣  @design-it/glassmorphism
    "Generate visual design: event card, listing, modal"

3️⃣  @design-system
    "Extract design tokens: colors, spacing, typography, variants"

4️⃣  @react-best-practices + @nextjs-app-router-patterns
    "Implement events listing with server components and Suspense"

5️⃣  @shadcn + @react-ui-patterns
    "Build EventCard, EventModal, RegistrationForm components"

6️⃣  @tanstack-query-expert
    "Wire up data fetching, caching, optimistic mutations"

7️⃣  @ui-motion + @animejs-animation
    "Add page transitions, card animations, success celebrations"

8️⃣  @web-performance-optimization
    "Optimize: images, code split, lazy load"

9️⃣  @accessibility-compliance-accessibility-audit
    "Audit and fix all WCAG AA issues"

🔟  @playwright-skill
    "Write E2E tests for all critical user flows"
```
