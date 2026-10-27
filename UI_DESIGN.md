# Mobile UI/UX Design Specification: Gaming Voice & Chat (MVP)

## 1. Design Philosophy
Mobile gamers have distinct ergonomic and performance needs:
- **Dark & High Contrast**: Optimized for OLED displays to save battery, with vivid accent indicators (Emerald for voice activity, Indigo for primary actions).
- **One-Thumb Reachability**: Critical navigation elements and bottom sheets sit within the lower half of the screen.
- **Minimal Cognitive Overhead**: Zero clutter, no heavy nested category menus, and frictionless onboarding.
- **In-Game Non-Intrusiveness**: Overlays must not block game controls (joysticks, aim/fire triggers) and must offer instant one-tap mute.

---

## 2. Core Screen Breakdown

### Screen 1: Frictionless Auth (`AuthScreen`)
- **Username**: Chosen gamer handle.
- **Avatar Selector**: Quick-pick gamer icons (🎮, ⚡, 🎯, 🐺).
- **Password**: Credential input.
- **No Email Wait**: Immediate entry to squad screens without email confirmation loops.

### Screen 2: Group & Channel Navigation (`ChannelListScreen`)
- **Left Rail**: Compact server/group icon strip (`APX`, `PUB`, `+` to join via code).
- **Channel List**:
  - `#` Text channels with unread badges.
  - `🔊` Voice channels showing active participants and live speaking indicators directly in the list.
- **Bottom User Dock**: Shows current user status, mute toggle, and settings shortcut.

### Screen 3: Instant Text Chat (`ChatScreen`)
- **Header**: Channel name, member count, and a persistent "🔊 Quick Join Voice" CTA button.
- **Message Stream**:
  - Instant render from local SQLite cache (zero spinners on cold start).
  - Background sync fetches messages since `last_message_id`.
- **Message Input Dock**: Clean text input with sticky send button.

### Screen 4: Active Voice Call Sheet (`VoiceRoomScreen`)
- **Participant Tiles**:
  - Active speaker tile highlighted with a pulsing green aura.
  - Muted / silent badges.
- **Bottom Action Bar**:
  - **Mute**: Microphone toggle.
  - **Deafen**: Silence all incoming audio.
  - **BT / Audio Route**: Earpiece, Speakerphone, or Bluetooth headset.
  - **Leave**: Instant call disconnect.
- **Technical Status**: Subtle indicator showing "Opus 24kbps • DTX Active • Low CPU".

### Screen 5: In-Game Floating HUD (`FloatingOverlayService`)
- **Permission**: Android `SYSTEM_ALERT_WINDOW`.
- **Floating Pill**:
  - Translucent draggable bubble positioned along screen edges.
  - Displays currently speaking teammate.
  - One-tap quick mute/unmute directly over games without minimizing.
- **Android Foreground Service**: Ongoing notification keeps WebRTC pipeline alive against aggressive OS memory killers.

---

## 3. Design System Tokens (Jetpack Compose)

| Token | Value / Role |
| :--- | :--- |
| **Background Primary** | `#0D0E12` (Ultra-dark surface) |
| **Background Secondary** | `#14161D` (Card / channel list surface) |
| **Background Elevated** | `#1C1E26` (Input field & dialog surface) |
| **Accent Primary** | `#4F46E5` (Indigo - Primary actions & buttons) |
| **Voice Active / Green** | `#10B981` (Emerald - Speaking indicator & online status) |
| **Danger / Disconnect** | `#E11D48` (Rose - Leave call / destructive action) |
| **Typography Minimum** | `12sp` body / captions; `16sp` titles; bold weights for gamer handles |
