# 📱 Workflow 02 — Build a Mobile App (React Native / iOS / Android)

This workflow walks you through building a cross-platform mobile app from architecture to App Store / Play Store launch.

---

## 🎯 Goal
Build and ship a production mobile app for iOS and Android.

**Example:** College Events mobile app (React Native + Expo)

---

## ⏱️ Estimated Timeline
| Phase | Time |
|---|---|
| Architecture & Design | 1-2 days |
| Core UI Screens | 3-5 days |
| API Integration & Auth | 2-3 days |
| Native Features | 2-3 days |
| Testing & QA | 2-3 days |
| Store Submission | 1-2 days |
| **Total** | **~2-3 weeks** |

---

## Phase 1 — Architecture & Design

### Step 1.1 — App Architecture
```
@react-native-architecture Design the architecture for our college events mobile app:
Platforms: iOS + Android
Backend: existing REST API at api.events.college.edu

Define:
- Navigation structure: which library? (Expo Router vs React Navigation)
- State management: Zustand (global) + TanStack Query (server state)
- Auth: JWT stored in SecureStore, Google OAuth via Expo AuthSession
- Offline: which screens work without internet? (Tickets must — QR needs to work offline)
- Push notifications: event reminders, waitlist alerts
- QR scanner: validate tickets at venue entrance
- Folder structure: feature-based (auth/, events/, tickets/, profile/)

Recommend tech stack with justification for each choice.
```

**Output:** Architecture document, tech stack decisions, folder structure

### Step 1.2 — UX Flow & Wireframes
```
@mobile-design Design the UX flow for our events mobile app:

Core screens (MVP):
1. Onboarding (3 slides): what the app does → sign in with Google
2. Home: personalized event feed + search bar
3. Events List: filterable by category, date, free/paid
4. Event Detail: full info + register button + map
5. Registration: form → payment → success
6. My Tickets: list of QR ticket cards (works offline)
7. Ticket Detail: large QR code + event info
8. Profile: name, college, interests, settings

For each screen:
- What's the primary action?
- What data is shown above the fold?
- Navigation: tab bar? header back? bottom sheet?

Apply iOS HIG and Material Design guidelines appropriately per platform.
```

**Output:** Screen flow diagram, wireframe descriptions

### Step 1.3 — Visual Design
```
@design-it/glassmorphism Apply glassmorphism style to our mobile app:
Key screens to design:
1. Home hero: blurred campus photo behind event cards
2. Event card: frosted glass card with image, title, date
3. Ticket card: glass card with QR code, event name, user name
4. Bottom tab bar: frosted glass with subtle blur

Mobile-specific:
- Safe area insets (iOS notch, Android status bar)
- Dark mode (system preference)
- Dynamic type support (accessibility font sizes)

Output: React Native StyleSheet tokens matching the design.
```

---

## Phase 2 — Project Setup

### Step 2.1 — Expo Project Init
```
@expo-dev-client Set up our Expo project:
- SDK: latest (SDK 52+)
- Template: tabs (Expo Router)
- TypeScript: strict mode
- EAS project linked
- Dev client: custom dev build (we need native modules)

Configure:
- app.json: name, bundle IDs (com.collegename.events)
- babel.config.js: module-resolver for @ imports
- tsconfig.json: strict, paths
- .env: EXPO_PUBLIC_API_URL, EXPO_PUBLIC_STRIPE_KEY
- eas.json: development, preview, production profiles

Verify: npx expo start --dev-client runs on both simulators
```

### Step 2.2 — Design System Setup
```
@design-system Set up the design token system for React Native:
Using: react-native-unistyles (or StyleSheet with constants)

Tokens to define:
- colors: primary, secondary, background (light/dark), text, border
- spacing: 4/8/12/16/24/32/48 scale
- typography: fontFamily (Inter), fontSizes, fontWeights, lineHeights
- borderRadius: sm/md/lg/full
- shadows: sm/md/lg (platform-specific: shadow on iOS, elevation on Android)

Export: a useTheme() hook that returns all tokens + respects system dark mode.
```

---

## Phase 3 — Core UI Screens

### Step 3.1 — Navigation Setup
```
@expo-ui Set up Expo Router navigation for our app:
File structure:
app/
  _layout.tsx          ← Root layout (auth guard, theme provider)
  (auth)/
    login.tsx          ← Google OAuth sign-in screen
    onboarding.tsx     ← 3-slide intro (shown once)
  (tabs)/
    _layout.tsx        ← Tab bar (Home, Explore, Tickets, Profile)
    index.tsx          ← Home feed
    explore.tsx        ← Events list with filters
    tickets.tsx        ← My tickets list
    profile.tsx        ← User profile
  events/
    [id].tsx           ← Event detail (pushed from Home or Explore)
  tickets/
    [id].tsx           ← Ticket detail with QR (pushed from tickets tab)

Tab bar icons: Home (house), Explore (search), Tickets (ticket), Profile (person)
Tab bar style: frosted glass with active tab indicator
```

### Step 3.2 — Core Components
```
@expo-ui Build the core reusable components:

1. EventCard
Props: event (id, title, date, image, category, spotsRemaining)
- expo-image for performant image loading + blur placeholder
- Category badge with color coding
- Date formatted: "Sat, Dec 15 · 6:00 PM"
- Spots remaining: "23 spots left" (red if < 5)
- Press: navigate to events/[id]
- Press animation: scale 0.97 on press (react-native-reanimated)

2. TicketCard
Props: ticket (id, event, qrData, used)
- Gradient background (event category colors)
- Event name, date, venue
- QR code (react-native-qrcode-svg)
- "Used" overlay if validated
- Works fully offline (QR data stored in local storage)

3. CategoryPill
Props: category, selected, onPress
- Horizontal scrollable list of pills
- Animated selection with spring (reanimated)

4. SkeletonCard
- Animated shimmer loading state for EventCard
- Uses Reanimated shared values for smooth shimmer
```

### Step 3.3 — Event Detail Screen
```
@swiftui-ui-patterns (for iOS) / @android-jetpack-compose-expert (for Android)
OR for cross-platform:
@expo-ui Build the Event Detail screen:

Layout:
- Hero: full-width image (height: 280), parallax on scroll (Reanimated)
- Back button: floating, frosted glass circle
- Content (scrollable):
  - Title (24px bold), Organizer name
  - Date/Time chip, Venue chip with map icon
  - Category badge
  - Description (collapsible after 3 lines)
  - Capacity: progress bar + "X spots remaining"
  - Organizer section with avatar

Bottom bar (sticky):
- Price (or "Free")
- "Register Now" CTA button (prominent, full width on mobile)
- Disabled + "Join Waitlist" when sold out

Accessibility: all tap targets ≥ 48pt, VoiceOver labels
```

---

## Phase 4 — API Integration & Auth

### Step 4.1 — Auth Implementation
```
@auth-implementation-patterns Implement auth in our Expo app:
Method: Google OAuth via Expo AuthSession

Flow:
1. Tap "Sign in with Google"
2. Open Expo AuthSession → Google OAuth consent screen
3. Receive auth code → exchange for Google tokens
4. Send Google ID token to our API (POST /auth/google)
5. API verifies token, creates/updates user, returns JWT pair
6. Store: accessToken in memory, refreshToken in SecureStore
7. Axios interceptor: auto-refresh on 401

Token refresh:
- On 401 response: pause queue, refresh token, retry original request
- If refresh fails (token expired/revoked): sign out, navigate to login

Sign out: clear SecureStore, clear memory, navigate to login
```

### Step 4.2 — API Client & Data Fetching
```
@tanstack-query-expert Set up TanStack Query for our React Native app:

API client: Axios with baseURL + auth interceptor
queryClient: configured with offline persistence (mmkv-based)

Queries:
- useEvents(filters): paginated events, staleTime: 5min
- useEvent(id): single event detail, prefetch on card render
- useMyTickets(): user's tickets, staleTime: 1min, offline: serve cached

Mutations:
- useRegisterForEvent():
  - Optimistic: show "Processing..." state immediately
  - Success: navigate to ticket screen, invalidate myTickets
  - Error: show error toast, restore previous state
- useJoinWaitlist(): add to waitlist, show position

Offline persistence:
- Tickets query: persist to MMKV (AsyncStorage alternative, fast)
- Events: persist for 30 minutes (stale-while-revalidate)
```

### Step 4.3 — Offline Ticket Support
```
@react-native-skills Implement offline QR ticket support:
Requirement: QR tickets must work without internet at venue

Strategy:
1. When ticket is loaded: store QR data in MMKV keyed by ticket_id
2. Ticket detail screen: load from MMKV first, then API (network-first with local fallback)
3. On successful registration: immediately persist ticket data locally
4. Detect offline: use @react-native-community/netinfo
5. UI: show "✓ Available offline" indicator on ticket card

QR code:
- Data: JWT signed by our server containing {ticketId, eventId, userId, exp}
- Display: react-native-qrcode-svg (works fully offline)
- Brightness: auto-increase screen brightness when ticket is open

Test: airplane mode → open app → view ticket → QR displays correctly.
```

---

## Phase 5 — Native Features

### Step 5.1 — QR Scanner (Ticket Validation)
```
@expo-module Implement QR scanner for organizer ticket validation:
Using: expo-camera

Scanner screen (organizer only):
1. Camera viewfinder (full screen)
2. Overlay: scanning frame animation (corners animate in/out)
3. On QR detected:
   a. Vibrate (short) + pause scanner
   b. Decode: verify JWT signature locally (no internet needed for basic check)
   c. If valid: call API to mark as used (POST /tickets/:id/validate)
   d. Success: green flash + "✓ [Name] — Valid" for 2 seconds
   e. Already used: orange flash + "Already Checked In"
   f. Invalid: red flash + "Invalid Ticket"
4. Auto-resume scanner after 2 seconds

Offline mode: validate JWT locally, sync "used" status when internet returns.
```

### Step 5.2 — Push Notifications
```
@expo-dev-client Set up push notifications with Expo:

Setup:
- Request permissions on first meaningful engagement (not on launch)
- Register token: POST /notifications/register with Expo push token
- Store token on our server per user

Notification types:
1. Event reminder: "🎵 [Event] starts in 1 hour" → open Event Detail
2. Waitlist alert: "✅ A spot opened up for [Event]! Register now" → open Event
3. Registration confirmed: "🎫 Your ticket for [Event] is ready" → open Ticket
4. New event: "🆕 [Event] just posted — only 20 spots" → open Event

Notification handling:
- App in foreground: in-app toast
- App in background/closed: OS notification → tap → deep link to correct screen

Deep links: events.college://events/:id, events.college://tickets/:id
```

### Step 5.3 — Maps Integration
```
@expo-ui Add venue maps to the Event Detail screen:
Library: react-native-maps (iOS: Apple Maps, Android: Google Maps)

On Event Detail:
- Show mini map (height: 150px) below venue name
- Marker on venue location
- Tap map → full screen map OR open native maps app (Directions)

Full screen map:
- Marker with event name callout
- User location (if permission granted)
- "Get Directions" button → open Apple Maps / Google Maps

Data: venue has lat/lng from organizer input
Geocoding: if only address, use Google Geocoding API to get lat/lng.
```

---

## Phase 6 — Testing

### Step 6.1 — Component Tests
```
@jest-skill Write component tests for our React Native components:
Library: @testing-library/react-native

Test: EventCard
- Renders with all props: title, date, image, spots
- Shows "Sold Out" when spotsRemaining === 0
- Shows "X spots left" in red when < 5 spots
- onPress fires with correct event ID

Test: TicketCard
- Renders QR code with correct data
- Shows "Used" overlay when ticket.used === true
- Works when offline (no network mock)

Test: Registration mutation
- Success: navigates to ticket screen
- Error: shows error message
- Loading: disables register button
```

### Step 6.2 — E2E Tests
```
@android-ui-journey-testing Write Detox E2E tests for critical flows:

Flow 1: Register for an event
1. Launch app (logged in as test student)
2. Tap first event card on Home
3. Tap "Register Now"
4. Fill registration form
5. Complete Stripe test payment
6. Assert: success screen with "You're In!"
7. Tap "View My Ticket"
8. Assert: QR code visible and scannable

Flow 2: Offline ticket access
1. Disable network (Detox: device.setStatusBar or mock)
2. Navigate to My Tickets
3. Assert: tickets loaded from cache
4. Tap ticket → assert QR code visible

Flow 3: QR scanner validation
1. Login as organizer
2. Navigate to Scanner tab
3. Simulate QR scan (mock the camera input)
4. Assert: "Valid" response shown
```

---

## Phase 7 — Store Submission

### Step 7.1 — Store Optimization
```
@app-store-optimization Optimize our events app listing:
Target: college students in India, 18-24

App Store (iOS):
- Name: "College Events — Campus Life" (30 chars max)
- Subtitle: "Find & Register for Events" (30 chars max)
- Keywords (100 chars): "college events,campus events,student activities,hackathon,cultural fest,registration"
- Description (4000 chars): lead with student benefit, then features, social proof, CTA
- Screenshots (6.7" + 5.5"): show Home, Event Detail, Ticket QR, Scanner
- Preview video: 30s showing full registration flow

Play Store (Android):
- Short description (80 chars): "Find campus events & register instantly"
- Full description (4000 chars): similar to App Store but Android tone
- Feature graphic: 1024x500px with key value proposition
```

### Step 7.2 — EAS Build & Submit
```
@expo-deployment Set up production deployment pipeline:

EAS Build:
# Production iOS build
eas build --platform ios --profile production

# Production Android build  
eas build --platform android --profile production

Signing:
- iOS: Distribution certificate + provisioning profile (auto-managed by EAS)
- Android: upload keystore (generate once, store securely)

EAS Submit:
# Submit to App Store Connect (TestFlight first)
eas submit --platform ios --profile production

# Submit to Play Store (Internal Testing first)
eas submit --platform android --profile production

Review timeline:
- iOS: 24-48 hours for initial review
- Android: 3-7 days for first submission

OTA Updates (post-launch):
eas update --branch production --message "Fix event detail loading"
# Ships to users within minutes, no store review needed for JS changes
```

---

## ✅ Launch Checklist

```
□ All E2E tests passing on iOS simulator + Android emulator
□ Tested on real device (iPhone + Android)
□ Offline mode: tickets work without internet
□ Push notifications: test all 4 notification types
□ QR scanner: tested with 10 different ticket QR codes
□ App Store: all screenshots uploaded, description reviewed
□ Privacy policy URL: linked in App Store listing
□ App review notes: explain any permissions needed (camera for QR)
□ TestFlight: 5+ internal testers approved before public release
□ Analytics: events firing correctly in PostHog
□ Crash reporting: Sentry configured + test crash verified
□ Support: FAQ page for app issues
```
