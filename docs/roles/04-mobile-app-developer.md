# 📱 Mobile App Developer — Skills Guide

Mobile app developers build for iOS, Android, and cross-platform targets. This guide covers React Native/Expo, native iOS (SwiftUI), native Android (Jetpack Compose), Flutter, and cross-platform deployment.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| React Native / Expo | `@expo-ui`, `@expo-api-routes`, `@react-native-architecture`, `@expo-deployment` |
| iOS / SwiftUI | `@ios-developer`, `@swiftui-expert-skill`, `@swift-concurrency-expert` |
| Android / Kotlin | `@android-dev`, `@android-jetpack-compose-expert`, `@kotlin-coroutines-expert` |
| Flutter | `@flutter-expert` |
| UI / Design | `@mobile-design`, `@hig-foundations`, `@building-native-ui`, `@iconsax-library` |
| Testing | `@android-ui-journey-testing`, `@appium-skill`, `@awt-e2e-testing` |
| Deployment | `@expo-deployment`, `@app-store-optimization`, `@eas-update-insights` |
| Security | `@mobile-security-coder` |

---

## ⚡ React Native & Expo

### `@react-native-architecture`
Project architecture decisions for React Native apps.
```
@react-native-architecture Design the architecture for a college events mobile app:
- Navigation: React Navigation or Expo Router?
- State: Zustand + React Query
- Data fetching: from our existing REST API
- Offline support strategy
- Push notifications
- QR scanner for ticket validation
Recommend folder structure and key packages.
```

### `@expo-ui`
Expo UI components, theming, and native UI patterns.
```
@expo-ui Build an EventCard component for Expo:
- Uses expo-image for optimized images
- Shows: title, date, location, spots remaining
- Animated press feedback with react-native-reanimated
- Skeleton loading state with expo-linear-gradient
- Supports both iOS and Android visual styles
```

### `@expo-api-routes`
Expo API routes for server-side logic in an Expo app.
```
@expo-api-routes Set up Expo API routes for:
- GET /api/events (list with pagination)
- POST /api/events/[id]/register (requires auth)
- GET /api/tickets/[id] (QR ticket data)
Include: middleware for JWT validation, error responses.
```

### `@expo-dev-client` / `@expo-module`
Custom native modules and expo-dev-client setup.
```
@expo-dev-client Set up expo-dev-client for our team:
- Install and configure
- Add custom native modules (BLE scanner for venue check-in)
- Build configuration for dev vs staging vs prod
- Team sharing via EAS
```

### `@expo-deployment` / `@eas-update-insights`
EAS Build, submit, and OTA updates.
```
@expo-deployment Set up production deployment pipeline:
- EAS Build profiles: development, preview, production
- EAS Submit to App Store and Play Store
- OTA updates with EAS Update
- Environment variables per build profile
- Automatic version bumping
```

### `@expo-cicd-workflows`
CI/CD for Expo apps with GitHub Actions.
```
@expo-cicd-workflows Create GitHub Actions workflow for our Expo app:
- On PR: run tests, type check, Expo export dry-run
- On merge to main: EAS Update (staging OTA)
- On release tag: EAS Build + EAS Submit (production)
```

### `@react-native-skills`
React Native patterns, performance, debugging.
```
@react-native-skills Our app has janky scrolling in the events list.
Diagnose and fix:
- FlatList vs FlashList comparison
- Getitem layout for fixed-height items
- keyExtractor optimization
- Image caching
- useMemo for renderItem
```

### `@upgrading-expo`
Migrating between Expo SDK versions.
```
@upgrading-expo We're on Expo SDK 50, upgrading to SDK 52.
Generate migration guide:
- Breaking changes affecting our packages
- Updated package versions
- Metro config changes
- New API usage we should adopt
```

---

## 🍎 iOS / SwiftUI

### `@ios-developer`
iOS development patterns, UIKit, Swift best practices.
```
@ios-developer Design the app architecture for our iOS events app:
- MVVM vs MVI for SwiftUI
- Navigation with NavigationStack
- Data layer: Combine or async/await?
- Keychain for token storage
- Background task for notification handling
```

### `@swiftui-expert-skill` / `@swiftui-ui-patterns`
SwiftUI views, modifiers, layout, state management.
```
@swiftui-expert-skill Build an EventDetailView in SwiftUI:
- Large header image with parallax
- Title, date, location with SF Symbols
- Capacity progress bar
- Register button with loading state
- Share sheet integration
- Supports dark mode and Dynamic Type
```

### `@swiftui-performance-audit`
SwiftUI performance — avoiding re-renders, list optimization.
```
@swiftui-performance-audit Profile and fix our EventsList view:
- Unnecessary body re-renders
- List cell performance (LazyVStack vs List)
- @StateObject vs @ObservedObject misuse
- Image loading blocking main thread
```

### `@swift-concurrency-expert`
Swift async/await, actors, structured concurrency.
```
@swift-concurrency-expert Rewrite our callback-based network layer using Swift concurrency:
- async/await for API calls
- Actor for thread-safe state management
- Task groups for parallel data fetching
- Proper cancellation on view dismiss
```

### `@hig-foundations` / `@hig-components-controls` / `@hig-patterns`
Apple Human Interface Guidelines compliance.
```
@hig-foundations Review our events app against Apple HIG:
- Are we using standard iOS navigation patterns?
- Touch targets meeting 44pt minimum?
- Correct use of SF Symbols?
- Dynamic Type support?
- Proper use of system colors and materials?
```

### `@swiftui-liquid-glass` / `@swiftui-view-refactor`
Modern SwiftUI patterns and visionOS-ready designs.
```
@swiftui-liquid-glass Redesign the registration success screen:
- Liquid glass card effect
- Ticket with mesh gradient background
- QR code with subtle glow
- Haptic feedback on appearance
```

### `@ios-debugger-agent`
iOS debugging — Memory Instruments, crash logs, network inspection.
```
@ios-debugger-agent Our app crashes intermittently on ticket scan.
Help analyze this crash report:
[paste crash log]
What's causing the crash? How do we reproduce and fix it?
```

### `@add-app-clip`
Adding an App Clip for quick event check-in.
```
@add-app-clip Add an App Clip to our events app:
- Target: QR scan at venue for quick check-in
- No full app install needed
- Passes venue name and event ID via URL
- Shows check-in confirmation
- Suggests full app install after
```

---

## 🤖 Android / Kotlin / Jetpack Compose

### `@android-dev`
Android development: Activities, Fragments, ViewModels.
```
@android-dev Design the Android architecture for our events app:
- MVVM with Android ViewModel
- Navigation Component for screen routing
- DataStore for preferences
- WorkManager for background tasks
- Hilt for dependency injection
```

### `@android-jetpack-compose-expert`
Jetpack Compose UI patterns.
```
@android-jetpack-compose-expert Build an EventCard composable:
- Card with image, title, date, capacity chip
- Click ripple effect
- Animated expand/collapse for description
- Proper Material3 theming
- Accessibility: semantics, content descriptions
```

### `@kotlin-coroutines-expert`
Kotlin coroutines, Flow, StateFlow.
```
@kotlin-coroutines-expert Implement the registration flow using coroutines:
- ViewModel: StateFlow<RegistrationUiState>
- Repository: suspend fun for API call
- Error handling with sealed Result class
- Loading/success/error states
- Cancellation when user leaves screen
```

### `@android-ui-journey-testing`
Android UI journey tests with Espresso or Compose testing.
```
@android-ui-journey-testing Write UI tests for the registration flow:
1. Navigate to events list
2. Click first event
3. Click Register
4. Fill form and submit
5. Assert success screen with ticket
Using Compose UI Testing APIs.
```

---

## 🐦 Flutter

### `@flutter-expert`
Flutter widgets, state management (BLoC/Riverpod), platform channels.
```
@flutter-expert Build the events app in Flutter:
- Architecture: Clean architecture with Riverpod
- Feature-based folder structure
- GoRouter for navigation
- API integration with Dio
- Local storage with Hive
- Push notifications with FCM
```

---

## 🎨 Mobile UI & Design

### `@mobile-design`
Mobile-first design principles and patterns.
```
@mobile-design Design the UX flow for our mobile events app:
- Onboarding (3 screens max)
- Home: personalized event recommendations
- Event detail: all info + register CTA prominent
- My Tickets: QR codes accessible without internet
- Profile: minimal, focused on what matters
Apply iOS and Material Design guidelines appropriately.
```

### `@building-native-ui`
Cross-platform native UI patterns.
```
@building-native-ui Implement a cross-platform QR scanner:
- Expo Camera (React Native)
- AVFoundation (iOS native)
- CameraX (Android)
Show the implementation for each, with fallback handling.
```

---

## 🔒 Mobile Security

### `@mobile-security-coder`
Mobile security best practices.
```
@mobile-security-coder Security review our mobile app:
- Token storage (Keychain/Keystore, not AsyncStorage)
- Certificate pinning setup
- Jailbreak/root detection
- Screenshot prevention on sensitive screens
- Deep link validation
- Biometric authentication implementation
```

---

## 📦 App Store & Deployment

### `@app-store-optimization`
ASO — keywords, screenshots, descriptions that convert.
```
@app-store-optimization Optimize our events app App Store listing:
- Title and subtitle with keywords
- Description (4000 chars): benefits-focused
- Screenshot strategy (6 screens)
- Keywords (100 chars): research competitors
- Preview video storyboard
Target: college students in India
```

---

## 🔗 Complete Mobile App Prompt Chain

```
1️⃣  @react-native-architecture (or @ios-developer / @android-dev)
    "Design app architecture: navigation, state, data, notifications"

2️⃣  @mobile-design
    "Design UX flow: onboarding, home, event detail, ticket"

3️⃣  @expo-ui (or @swiftui-expert-skill / @android-jetpack-compose-expert)
    "Build core UI components: EventCard, TicketView, QR Scanner"

4️⃣  @expo-api-routes (or backend API integration)
    "Connect to REST API: events, registration, auth"

5️⃣  @auth-implementation-patterns
    "Implement login: OAuth, JWT, secure storage"

6️⃣  @mobile-security-coder
    "Security review: token storage, certificate pinning, input validation"

7️⃣  @android-ui-journey-testing (or @appium-skill)
    "Write UI journey tests for critical flows"

8️⃣  @expo-cicd-workflows (or @expo-deployment)
    "Set up CI/CD: automated builds, OTA updates, store submission"

9️⃣  @app-store-optimization
    "Optimize App Store / Play Store listing for discovery"
```
