# 🎮 Game Developer — Skills Guide

Game developers build interactive experiences — 2D, 3D, multiplayer, VR/AR, mobile, and web games. This guide covers Unity, Unreal, Godot, web games, shaders, and game design.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Unity | `@unity-developer`, `@unity-ecs-patterns`, `@unity-ai-game-creator` |
| Unreal | `@unreal-engine-cpp-pro` |
| Godot | `@godot-gdscript-patterns`, `@godot-4-migration`, `@bevy-ecs-expert` |
| Genres | `@game-development/2d-games`, `@game-development/3d-games`, `@game-development/multiplayer` |
| VR / AR | `@game-development/vr-ar` |
| Mobile Games | `@game-development/mobile-games` |
| Web Games | `@game-development/web-games`, `@threejs-fundamentals`, `@threejs-animation` |
| Graphics / Shaders | `@shader-programming-glsl`, `@threejs-shaders`, `@threejs-materials` |
| Art & Audio | `@game-development/game-art`, `@game-development/game-audio` |
| Game Design | `@game-development/game-design` |

---

## 🟣 Unity

### `@unity-developer`
Unity core: scene management, MonoBehaviours, prefabs, physics, input.
```
@unity-developer Build a campus map exploration game for event discovery:
Core mechanics:
- 3D campus environment (low-poly aesthetic)
- Player controller: WASD movement + mouse look (first person)
- Event markers: glowing orbs on map at real event locations
- Interaction system: press E near marker to open event info
- UI: world-space canvas card showing event title, date, spots remaining
- Mobile support: touch controls with virtual joystick

Provide:
- Scene hierarchy structure
- Player controller MonoBehaviour (CharacterController)
- EventMarker.cs with OnTriggerEnter and canvas activation
- InputManager setup for keyboard + touch
```

### `@unity-ecs-patterns`
Unity DOTS (Data-Oriented Technology Stack) for high-performance entity management.
```
@unity-ecs-patterns Optimize our campus map game using Unity DOTS/ECS:
Problem: 500 event marker GameObjects cause 15ms update frames on mobile

Convert to ECS:
- EventMarkerComponent: position, eventId, capacity, isActive
- EventMarkerRenderSystem: update renderer colors based on capacity %
  (green = available, yellow = < 20% left, red = sold out)
- PlayerProximitySystem: find nearest 5 markers to player using BVH
- Burst-compiled: use [BurstCompile] on all jobs
- Job: IJobForEach for batch marker updates

Target: < 2ms update time on mid-range Android
```

### `@unity-ai-game-creator`
AI-driven NPC behavior and procedural game content.
```
@unity-ai-game-creator Add an AI campus guide NPC to our game:
- NPC wanders the campus on a NavMesh
- Has a knowledge base of all campus events
- Player can approach and ask: "What events are happening today?"
- NPC responds with spoken dialogue + points to nearest marker
- Uses Unity's ML-Agents for realistic wandering behavior
- Conversation system: keyword matching + LLM fallback (GPT-4o API call)
```

---

## 🔵 Unreal Engine

### `@unreal-engine-cpp-pro`
Unreal Engine C++: Actors, Components, Blueprints, GAS.
```
@unreal-engine-cpp-pro Build an interactive campus venue tour in Unreal Engine:
- APlayerCharacter: first-person camera, smooth movement
- AEventInfoActor: placed at event venues, shows info widget on overlap
- UEventDataComponent: stores event JSON data, fetches from REST API
- Widget Blueprint: styled UMG widget for event details
- Cinematic: level sequence flythrough triggered on game start
- Optimization: LOD groups for campus buildings, occlusion culling
```

---

## 🟢 Godot

### `@godot-gdscript-patterns`
Godot 4 with GDScript: nodes, signals, resources, scene tree.
```
@godot-gdscript-patterns Build an "Event Planner" puzzle game in Godot 4:
Game concept: drag-and-drop events into venue slots, maximize student happiness

Nodes and scenes:
- Main.tscn: game root, score display, time limit
- EventCard.tscn: draggable card with title, duration, capacity, type
- VenueSlot.tscn: drop target with validation rules (capacity, time conflict)
- ScoreManager.gd: autoload singleton for tracking score

GDScript patterns:
- Signals: event_placed(event_id, venue_id), conflict_detected()
- Resource: EventData (type, duration, happiness_value)
- Use @export for inspector-editable properties
- Connect signals in _ready() not in editor for dynamic slots
```

### `@godot-4-migration`
Migrating a Godot 3 project to Godot 4.
```
@godot-4-migration Migrate our Godot 3 campus game to Godot 4:
Breaking changes affecting our project:
- yield() → await
- connect() syntax changes
- setget → @property
- KinematicBody → CharacterBody3D
- Navigation: NavigationServer3D API changes
- GDNative → GDExtension (we have a C++ plugin)

Give me: migration script for automatic conversions + manual change checklist.
```

### `@bevy-ecs-expert`
Bevy game engine (Rust-based ECS) for performance-critical games.
```
@bevy-ecs-expert Build the core game loop in Bevy ECS:
Entities: EventMarker, Player, UICard
Components: Transform, EventData { id, name, capacity }, Visibility
Systems:
- spawn_markers: reads event JSON, spawns marker entities
- update_marker_colors: changes material based on capacity %
- detect_player_proximity: finds markers within 5 units of player
- show_event_card: spawns UI entity on proximity trigger
Resources: EventDatabase (HashMap<EventId, EventData>)
Run systems: Update schedule with ordering constraints
```

---

## 🌐 Web Games

### `@game-development/web-games`
Browser-based games with Phaser.js or raw Canvas.
```
@game-development/web-games Build a campus events trivia game in Phaser 3:
Game flow:
- Intro screen: "How well do you know campus events?" + Start button
- Question screen: 10 questions about past campus events, 30s timer per question
- Multiplayer: up to 4 players via WebSocket room
- Power-ups: 50/50, extra time, steal points
- Results: scoreboard with share button

Tech:
- Phaser 3 for game scenes (GameScene, UIScene as overlay)
- Colyseus for real-time multiplayer server
- Tween animations for correct/wrong feedback
- Responsive: fits 360px mobile to 1920px desktop
```

### `@threejs-fundamentals` / `@threejs-animation`
Three.js 3D web experiences.
```
@threejs-fundamentals Create a 3D event ticket viewer in Three.js:
- Rectangular ticket mesh with event details texture
- Holographic foil shader effect on ticket surface
- Slow rotation animation (auto-rotate)
- Mouse hover: speed up rotation
- QR code texture on back side (flip animation)
- Point lights for dramatic lighting
- Postprocessing: bloom on foil effect
Embed in our React app (use @react-three/fiber).
```

---

## ✨ Shaders & Graphics

### `@shader-programming-glsl`
GLSL vertex and fragment shaders.
```
@shader-programming-glsl Write GLSL shaders for our campus game:

Shader 1: EventMarker Glow
- Fragment shader: pulsing outer glow
- Uniform: float time (from game clock)
- Effect: sin(time * 2.0) drives alpha and scale of outer ring
- Color: lerp between green and yellow based on capacity uniform

Shader 2: Capacity Heatmap
- Vertex shader: standard pass-through
- Fragment shader: color gradient red → yellow → green
- Uniform: float fill_percent [0.0, 1.0]
- Apply to venue floor mesh

Shader 3: Day/Night Sky
- Procedural sky: gradient from daylight blue to sunset orange to night purple
- Uniform: float time_of_day [0.0, 24.0]
```

### `@threejs-shaders` / `@threejs-materials`
Three.js custom ShaderMaterial and material configuration.
```
@threejs-shaders Create a holographic ticket shader in Three.js:
THREE.ShaderMaterial with:
- vertexShader: standard, pass UVs
- fragmentShader: 
  - Rainbow iridescence: hue shifts based on view angle (dot(normal, viewDir))
  - Scanlines: horizontal lines using sin(uv.y * 100)
  - Glitch noise: occasional random pixel displacement
  - Base texture: event ticket image underneath
Animate: update time uniform in animation loop
```

---

## 🎯 Genre-Specific Skills

### `@game-development/multiplayer`
Multiplayer architecture: networking, state sync, authority, lag compensation.
```
@game-development/multiplayer Design multiplayer for our campus exploration game:
Feature: see other online students on the campus map in real-time

Architecture decision: peer-to-peer vs server-authoritative?
For our use case (cosmetic sync only): recommend and explain.

Implementation:
- Use Photon Unity Networking (PUN2) or Mirror
- Sync per player: position, avatar color, username
- Area of interest: only receive updates from players in same zone
- Max: 50 players per campus server instance
- Reconnection: rejoin same room on disconnect
```

### `@game-development/vr-ar`
VR/AR development with Unity XR or WebXR.
```
@game-development/vr-ar Build an AR event overlay for our mobile app:
Feature: point phone camera at campus buildings → see event markers in AR

Tech: Unity AR Foundation + ARCore (Android) / ARKit (iOS)
Implementation:
- Plane detection: detect ground plane for marker placement
- Image tracking: detect event posters → show digital overlay
- World anchors: place event info cards at building locations
- UI: tap marker to expand full event details
- Performance: 60fps on mid-range phone
```

### `@game-development/mobile-games`
Mobile game optimization and monetization.
```
@game-development/mobile-games Optimize our campus game for mobile:
Target: smooth 60fps on Android (mid-range, 2022) + iOS (iPhone 12)

Optimization areas:
- Draw calls: batching, GPU instancing for event markers
- Texture compression: ASTC for iOS, ETC2 for Android
- Level of detail (LOD): campus buildings at 3 LOD levels
- Memory: texture atlases, addressable asset system
- Battery: adaptive FPS (30fps when player idle, 60fps when moving)
- Input: touch controls with haptic feedback
```

---

## 🎨 Art & Audio

### `@game-development/game-art`
Art direction, asset pipeline, style guides.
```
@game-development/game-art Define the art style for our campus game:
Style: low-poly stylized (similar to Monument Valley + Firewatch)
Color palette: warm campus tones (brick red, green grass, clear sky blue)
Event markers: simple geometric icons per category (music note, trophy, book)

Asset pipeline:
- 3D models: Blender → FBX → Unity
- Textures: 512px max for environment, 256px for props
- Atlas: combine small textures into 2048x2048 atlas
- Vertex colors: use instead of textures for terrain
```

### `@game-development/game-audio`
Sound design and audio implementation.
```
@game-development/game-audio Design the audio for our campus game:
Ambient:
- Campus ambience loop: birds, distant chatter, wind
- Zone-based: near library (quiet), near cafeteria (busier)

SFX:
- Event marker hover: soft chime (pitch varies by capacity)
- Registration success: uplifting 3-note jingle
- Footsteps: 4 variants for grass, concrete, indoor, stairs

Music:
- Upbeat lo-fi hip-hop for exploration
- Tense: when registration closes soon
Implementation: Unity Audio Mixer with 3D spatial audio for ambient sources.
```

---

## 🔗 Complete Game Developer Prompt Chain

```
1️⃣  @game-development/game-design
    "Design core loop, mechanics, win/fail states, progression"

2️⃣  @unity-developer (or @godot-gdscript-patterns / @unreal-engine-cpp-pro)
    "Set up engine project, scene hierarchy, core systems"

3️⃣  @unity-ecs-patterns
    "Optimize performance-critical systems with DOTS/ECS"

4️⃣  @shader-programming-glsl (or @threejs-shaders)
    "Add visual polish: glow effects, heatmaps, animated materials"

5️⃣  @game-development/multiplayer
    "Add social features: see other players, shared leaderboard"

6️⃣  @game-development/mobile-games
    "Mobile optimization: draw calls, LOD, battery, touch input"

7️⃣  @game-development/game-art
    "Define art pipeline and style guide"

8️⃣  @game-development/game-audio
    "Add ambient audio, SFX, and adaptive music"
```

---

## 💡 Pro Tips for Game Developers

1. **`@game-development/game-design` before any code** — nail the core loop first
2. **`@unity-ecs-patterns` when you have > 100 similar entities** — saves massive CPU
3. **`@shader-programming-glsl` is the fastest way to add visual polish** — one shader can transform the feel
4. **`@game-development/mobile-games` early** — optimizing at the end is 10x harder
5. **Chain `@game-development/game-audio` last** — sound design elevates a game from good to great
