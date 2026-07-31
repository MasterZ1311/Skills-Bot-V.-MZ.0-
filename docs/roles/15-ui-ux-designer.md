# 🎭 UI/UX Designer — Skills Guide

UI/UX designers craft the look, feel, and flow of digital products. This guide covers design process, visual styles, design systems, conversion optimization, accessibility, motion design, and UX writing.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Design Process | `@design-thinking`, `@design-ux`, `@ux-audit`, `@ux-flow` |
| Design Systems | `@design-system`, `@ui-tokens`, `@ui-skills`, `@baseline-ui` |
| Visual Styles | `@design-it/*` (40+ styles), `@high-end-visual-design`, `@antigravity-design-expert` |
| CRO | `@cro`, `@form-cro`, `@onboarding-cro`, `@page-cro`, `@popup-cro`, `@paywall-upgrade-cro` |
| Motion | `@ui-motion`, `@magic-animator`, `@review-animations`, `@fixing-motion-performance` |
| Accessibility | `@fixing-accessibility`, `@ui-a11y`, `@wcag-audit-patterns`, `@screen-reader-testing` |
| UX Copy | `@ux-copy`, `@copywriting`, `@beautiful-prose` |
| Prototyping | `@prototype`, `@canvas-design`, `@figma-automation` |

---

## 🧠 Design Process

### `@design-thinking`
Run structured design thinking sessions — from empathy to prototype.
```
@design-thinking Run a design thinking sprint for our event registration flow:
Problem: 40% of students abandon registration before completing payment.

Phase 1 — Empathize:
"Walk me through the emotional state of a student who just found out their
favorite event is nearly full and they're trying to register quickly."

Phase 2 — Define:
"Based on this, what is the core problem statement? Frame it as a HMW 
(How Might We) question."

Phase 3 — Ideate:
"Generate 10 solutions without filtering (include wild ideas)."

Phase 4 — Prototype:
"Pick the top 2 solutions. What's the simplest prototype for each?"

Phase 5 — Test:
"Write 5 questions to ask in a usability test session."
```

### `@ux-audit`
Audit an existing interface for usability issues.
```
@ux-audit Audit our event detail page:
[describe or paste screenshot URL]

Evaluate against:
- Nielsen's 10 Usability Heuristics
- Information hierarchy (does the most important info stand out?)
- Visual weight of CTA vs surrounding elements
- Cognitive load (how many decisions does the user have to make?)
- Friction points (what requires unnecessary effort?)
- Mobile vs desktop differences

Output: prioritized list of issues (Critical / Major / Minor) with specific fixes.
```

### `@ux-flow`
Map the ideal user journey for a feature.
```
@ux-flow Map the complete UX flow for a student discovering and registering for an event:

Start: student opens app for the first time this week
End: student has a confirmed ticket and knows where to show up

Map each step:
- User action
- Screen / UI state shown
- User's emotional state at this step
- Potential drop-off risk
- Micro-interaction or feedback that reassures the user

Highlight the 3 highest-risk steps where we're most likely to lose users.
```

---

## 🎨 Design Systems

### `@design-system`
Create a complete, scalable design system.
```
@design-system Build a design system for our events platform:

1. Foundation tokens:
   - Colors: primary (#6C63FF purple), secondary (#FF6584 pink), 
     neutral grays, semantic (error red, success green, warning amber)
   - Typography: Inter font family, scale (12/14/16/20/24/32/48px)
   - Spacing: 4px base grid (4, 8, 12, 16, 24, 32, 48, 64)
   - Radius: sm (4px), md (8px), lg (16px), full (9999px)
   - Shadows: sm, md, lg, xl

2. Component primitives:
   - Button: variants (primary, secondary, ghost, destructive) + sizes
   - Input: default, focused, error, disabled states
   - Card: default, interactive, elevated variants
   - Badge: category labels with color coding
   - Modal: with overlay, animation, focus trap
   - Toast: success, error, warning, info variants

3. Dark mode: full token mapping for dark theme
Output: CSS custom properties + Tailwind extend config.
```

### `@ui-tokens`
Design token management and theming.
```
@ui-tokens Set up a design token system for multi-brand support:
Our platform will be white-labeled for different colleges.
Each college has its own brand colors.

Token structure:
- Global tokens: primitive colors (all hex values)
- Alias tokens: semantic mappings (--color-primary → --color-college-primary)
- Component tokens: per-component overrides

Build:
- tokens.json in design tool format
- CSS custom properties output
- TypeScript constants for runtime theming
- How to swap brand at runtime with a CSS class
```

---

## 🎭 Visual Design Styles (`@design-it/*`)

The `design-it` skill family has **40+ visual style sub-skills**. Each transforms your UI with a distinct aesthetic.

### Complete Style Reference Table

| Style | Skill | Best For | Key Characteristics |
|---|---|---|---|
| Glassmorphism | `@design-it/glassmorphism` | SaaS dashboards, overlays | Frosted glass, backdrop blur, translucent cards |
| Neumorphism | `@design-it/neumorphism` | Premium apps, iOS feel | Soft shadows, extruded surfaces, minimal color |
| Neo-Brutalism | `@design-it/neo-brutalism` | Bold marketing, startups | Thick borders, flat colors, high contrast |
| Dark Mode | `@design-it/dark-mode` | Dev tools, pro apps | Dark backgrounds, muted colors, high contrast text |
| Minimalism | `@design-it/minimalism` | Content sites, productivity | White space, typography-driven, no decoration |
| Material Design | `@design-it/material-design` | Android, Google ecosystem | Elevation, ripple effects, system fonts |
| Cyberpunk | `@design-it/cyberpunk-ui` | Gaming, tech products | Neon accents, grid overlays, dark + electric |
| Synthwave | `@design-it/synthwave` | Entertainment, creative | Retro-futuristic, purple/pink neon, 80s grid |
| Retro | `@design-it/retro-design` | Nostalgia brands | Pixel fonts, muted palette, vintage texture |
| Y2K | `@design-it/y2k-design` | Fashion, trend brands | Chrome, hot pink, star bursts, glossy |
| Swiss Design | `@design-it/swiss-design` | Editorial, typography | Grid system, Helvetica, clean geometry |
| Vaporwave | `@design-it/vaporwave` | Art projects, albums | Pastels, Greek statues, grid, glitch |
| Holographic | `@design-it/holographic-ui` | Futuristic, metaverse | Rainbow iridescent, chrome surfaces |
| Sci-Fi | `@design-it/sci-fi-interface` | Data dashboards, HUDs | Terminal green, scanlines, hex grids |
| Claymorphism | `@design-it/claymorphism` | Friendly, playful apps | Puffy 3D shapes, pastel, rounded |
| Organic | `@design-it/organic-design` | Health, wellness, nature | Blob shapes, earthy tones, soft edges |
| Luxury | `@design-it/luxury-brand` | Premium, high-end | Gold, serif fonts, elegant spacing |
| Corporate | `@design-it/corporate-design` | Enterprise, B2B | Conservative, professional, trustworthy |
| Playful | `@design-it/playful-design` | Kids, games, education | Bright colors, cartoonish, bouncy |

### Example Invocation:
```
@design-it/glassmorphism Design a complete event registration modal:
Components to design:
- Modal container: frosted glass background (rgba + backdrop-filter)
- Event summary header: image with gradient overlay
- Form fields: glass input fields with subtle borders
- Price summary: elevated glass card
- CTA button: solid gradient (breaks from glass to draw attention)
- Close button: translucent X in corner

Color palette: purple (#6C63FF) to blue (#4ECDC4) gradient
Background: blurred dark campus photo
Output: complete HTML + CSS with working glassmorphism effects.
```

### `@high-end-visual-design`
Apple/Stripe-level premium UI design.
```
@high-end-visual-design Design a hero section for our events platform that feels premium:
- Large, confident typography (bold display font, 72px+ heading)
- Subtle animated gradient mesh background
- Social proof: "10,000 students · 500 events · 50 colleges"
  with smooth counter animation
- CTA button: solid, full-width on mobile, with arrow that moves right on hover
- Sub-text: short, benefit-focused (not feature list)

Feel: confident, modern, as polished as Stripe or Linear.
Output: HTML + CSS with specific font choices, exact color values, animations.
```

### `@magic-ui-generator`
Generate animated, scroll-driven UI sections quickly.
```
@magic-ui-generator Generate a features showcase section:
Features to showcase:
1. One-click registration
2. QR ticket delivery
3. Real-time capacity tracking
4. Analytics for organizers

Design:
- Cards that animate in on scroll (Intersection Observer)
- Icon animation on hover (slight bounce or rotate)
- Alternating layout: icon left / right
- Gradient accent lines connecting features
Output: self-contained HTML + CSS + JS, no framework needed.
```

---

## 📈 Conversion Rate Optimization (CRO)

### `@cro`
Optimize any page or flow for higher conversion.
```
@cro Optimize our event detail page (current: 15% view-to-register rate, target: 25%):

Apply CRO principles:
1. Above-fold audit: is the CTA visible without scrolling on mobile?
2. Social proof: add "342 students registered" and photos of past attendees
3. Scarcity: "Only 8 spots left" with urgency (if < 20% capacity)
4. Trust signals: organizer verified badge, past event star ratings
5. Friction removal: can we pre-fill name/email from profile?
6. Visual hierarchy: does the eye flow naturally to the CTA?
7. Mobile CTA: fixed sticky button at bottom on mobile?

Show: specific HTML/CSS changes for each improvement.
```

### `@form-cro`
Optimize forms for maximum completion rate.
```
@form-cro Optimize our 8-field registration form (30% completion → target 60%):

Current fields: name, email, phone, college, year, department, t-shirt size, dietary preference

Audit:
- Which fields are truly required? (Drop phone if not used)
- Can we pre-fill: name, email, college from user profile?
- Multi-step vs single page? (Show research-backed recommendation)
- Inline validation: show errors as user types, not on submit
- Progress indicator for multi-step
- CTA copy: "Complete Registration" vs "Claim My Spot"
- Payment section: add card logos, lock icon, "Secure payment" text

Show: redesigned form wireframe + specific copy changes.
```

### `@onboarding-cro`
Optimize the new user onboarding flow.
```
@onboarding-cro Our new user retention is low (Day 7: 22%, target: 40%).
Most users sign up but never register for an event.

Design an activation flow:
- Onboarding screen 1: personalize (pick 3 interest categories)
- Onboarding screen 2: show 3 events matching interests
- Onboarding screen 3: "Your first event is free — register now"
- Empty state: never show empty events list to new users

Nudges:
- Push notification: "New [interest] event: [title]" (Day 2 if no registration)
- Email: "3 events happening this week" (Day 3)
- In-app: persistent banner until first registration

Success metric: time to first registration < 24 hours from signup.
```

### `@paywall-upgrade-cro`
Optimize the moment users hit a paid-feature gate.
```
@paywall-upgrade-cro Design the upgrade prompt for organizers hitting the free tier limit:
Trigger: organizer tries to create 4th event (free tier: 3 max)

Upgrade prompt design:
- Don't just say "Upgrade to Pro" — show what they unlock
- Show the exact feature they tried to use
- Price anchor: "Less than ₹33/day"
- Social proof: "500+ active organizers on Pro"
- Risk removal: "Cancel anytime, no long-term commitment"
- CTA: "Upgrade and Create Event" (not "See Plans")

A/B test: modal vs inline upsell in the event creation form.
```

---

## ✨ Motion & Animations

### `@ui-motion`
Principled motion design using animation fundamentals.
```
@ui-motion Design the motion system for our events platform:
Based on the 12 principles of animation:

Page transitions:
- Route change: content slides in from right (300ms, ease-out)
- Back navigation: slide from left (250ms)

Micro-interactions:
- Button press: scale to 0.97 (100ms) → release back (150ms)
- Input focus: border width 1px → 2px with color change (150ms)
- Success checkmark: draw SVG path stroke (400ms, spring)
- Error shake: 3 horizontal nudges (300ms total)

Entrance animations:
- Cards: fade + translate Y from 20px → 0 (staggered 50ms)
- Modal: scale 0.9 → 1 + fade in (200ms, ease-out)

Reduced motion: respect prefers-reduced-motion, disable all non-essential animations.
```

### `@magic-animator`
Complex multi-step animation sequences.
```
@magic-animator Create the ticket reveal animation after successful registration:
Sequence (total: 1.5 seconds):
1. Screen darkens (200ms)
2. Ticket slides up from bottom (300ms, spring physics)
3. Confetti burst from top (500ms, particle system)
4. QR code draws in with a line scan effect (400ms)
5. "You're In! 🎉" text types out character by character (300ms)
6. Haptic feedback on mobile at step 2 and 5

Output: complete CSS keyframes + JS animation controller.
No external library dependency.
```

### `@review-animations`
Audit existing animations for quality and performance.
```
@review-animations Review the animations on our event listing page:
[describe current animations]

Check for:
- Performance: are we animating layout-triggering properties? (width, height → use transform instead)
- Jank: smooth 60fps or dropping frames? (use Chrome DevTools Performance tab)
- Duration: are they too fast (< 100ms feels invisible) or too slow (> 500ms feels sluggish)?
- Easing: are we using linear (robotic) when we should use ease-out or spring?
- Accessibility: prefers-reduced-motion honored?
- Overuse: are there too many animations happening simultaneously (cognitive overload)?
```

---

## ♿ Accessibility

### `@fixing-accessibility`
Fix specific accessibility issues.
```
@fixing-accessibility Fix all accessibility issues in our EventCard component:
[paste component HTML]

Issues to fix:
1. Image: alt text missing or uninformative
2. Button: no accessible name (using icon only)
3. Color: "Sold Out" badge in gray on white (fails contrast)
4. Interactive: click handler on a div, not a button
5. Focus: no visible focus indicator
6. Screen reader: date format announced as "2025-12-31" not "December 31st, 2025"

Show: corrected HTML for each issue + the WCAG criterion it violates.
```

### `@ui-a11y`
Build accessible components from the start.
```
@ui-a11y Design an accessible date/time picker for event registration:
Requirements:
- Calendar grid: arrow key navigation between days
- Screen reader: announces day, month, availability ("December 15, available")
- Focus trap: when calendar open, Tab stays within
- Keyboard: Enter selects, Escape closes, Home/End go to first/last day
- ARIA: role="grid", role="gridcell", aria-selected, aria-label on navigation buttons
- Color: selected state doesn't rely only on color
- Mobile: native date input fallback on touch devices
```

---

## ✍️ UX Copy

### `@ux-copy`
Write copy that guides, reassures, and converts.
```
@ux-copy Write UX copy for all states in our registration flow:

Empty states:
- "No events match your filters" → [helpful, suggests action]
- "You haven't registered for any events yet" → [inviting, not accusatory]

Error states:
- "Event is full" → [empathetic, offers waitlist]
- "Payment failed" → [clear cause + next step, no blame]
- "Registration deadline passed" → [honest, helpful alternative]

Success states:
- "You're registered!" → [celebratory, includes what happens next]
- "Added to waitlist" → [sets expectations, position shown]

CTA variants (A/B test these):
- "Register Now" vs "Claim Your Spot" vs "Get Your Ticket"
- "Join Waitlist" vs "Get Notified When Available"

Voice: friendly, clear, action-oriented. No jargon.
```

---

## 🔗 Complete UI/UX Designer Prompt Chain

```
1️⃣  @design-thinking
    "Run empathy → define → ideate → prototype sprint for registration flow"

2️⃣  @ux-flow
    "Map end-to-end user journey: discovery → ticket confirmation"

3️⃣  @design-it/glassmorphism (choose your style)
    "Generate visual design: color palette, typography, component look"

4️⃣  @design-system
    "Extract design tokens and component primitives"

5️⃣  @ux-copy
    "Write all UI copy: labels, errors, empty states, CTAs"

6️⃣  @cro
    "Optimize event detail page for higher registration conversion"

7️⃣  @form-cro
    "Optimize registration form: reduce fields, add inline validation"

8️⃣  @ui-motion
    "Design motion system: transitions, micro-interactions, entrance"

9️⃣  @magic-animator
    "Create ticket reveal celebration animation"

🔟  @wcag-audit-patterns → @fixing-accessibility
    "Audit and fix all WCAG AA accessibility issues"
```

---

## 💡 Pro Tips for UI/UX Designers

1. **`@design-thinking` before any wireframes** — validate you're solving the right problem
2. **`@design-it/*` family is your palette** — pick one style and stick to it for consistency
3. **`@cro` and `@form-cro` are pure gold for product metrics** — small copy/layout changes = big conversion lifts
4. **`@ui-motion` before `@magic-animator`** — establish principles before building complex sequences
5. **`@fixing-accessibility` immediately after building** — accessibility debt compounds quickly
6. **`@ux-copy` deserves as much thought as visual design** — words are UI too
