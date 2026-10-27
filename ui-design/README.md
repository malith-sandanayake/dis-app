# GameVoice — Apple Human Interface Design & Component Spec

This directory contains the Apple-inspired mobile UI/UX reference prototype, Jetpack Compose / Swift component architecture, and design tokens for **GameVoice** (MVP).

## Live Prototype
Open [`index.html`](file:///c:/Users/USER/Desktop/projects/dis-app/ui-design/index.html) in any modern browser.

---

## Apple Design Philosophy Applied

1. **San Francisco Typography & Optical Hierarchy:**
   * Font stack: `-apple-system, BlinkMacSystemFont, "SF Pro Display", "SF Pro Text", "Helvetica Neue", sans-serif`.
   * Clear tracking (`tracking-tight` for titles, `tracking-wider` uppercase section captions).
2. **Materials & Vibrancy (Frosted Glass / Translucency):**
   * Multi-layered dark blur materials (`backdrop-blur-xl`, `backdrop-blur-md`) with ultra-fine specular hairline borders (`border-white/[0.08]`).
   * Eliminates harsh borders and flat blocky designs in favor of depth and light absorption.
3. **Inset Grouped Layouts (iOS Pattern):**
   * Channels and settings organized into smooth rounded cards (`rounded-2xl`, `#1C1C1E`) with subtle dividers (`divide-white/[0.06]`), matching Apple Settings and Apple Messages.
4. **iMessage-Style Chat Bubbles:**
   * Inverted fluid message stream. Self messages feature Apple System Blue (`#007AFF`) with rounded corners; incoming messages render in soft dark slate (`#2C2C2E`).
5. **FaceTime Studio Voice Controls:**
   * Squircle participant tiles with subtle breathing green halo (`#34C759`) for voice activity detection (VAD).
   * Floating bottom action bar with circular frosted controls (Mute, Deafen, AirPods/AirPlay Route, and End Call in Apple Red `#FF3B30`).
6. **Live Activity Style In-Game HUD:**
   * Dynamic Island / Live Activity style floating translucent pill. Stays docked cleanly at the edge of active 3D games without blocking thumbsticks or action triggers.

---

## Jetpack Compose & Theme Token Mappings

| Apple Token | Color Hex | Role in Compose Theme |
| :--- | :--- | :--- |
| **System Black** | `#000000` | `MaterialTheme.colorScheme.background` (True AMOLED) |
| **Secondary Fill / Card** | `#1C1C1E` | `MaterialTheme.colorScheme.surface` (Inset cards) |
| **Tertiary Fill** | `#2C2C2E` | Incoming chat bubbles & inactive action discs |
| **System Blue (Tint)** | `#007AFF` | Primary CTA, outgoing message bubbles, active badges |
| **System Green (VAD)** | `#34C759` | Active speaker aura ring & online status |
| **System Red (Destructive)**| `#FF3B30` | End Call / Leave room |
| **Hairline Border** | `rgba(255,255,255,0.08)` | Delicate depth borders on frosted cards |
