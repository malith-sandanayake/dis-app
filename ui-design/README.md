# GameVoice UI/UX Architecture & Prototype Reference

This directory contains the mobile UI/UX interactive reference prototype and Jetpack Compose component breakdown for the **GameVoice** MVP.

## Quick View
Open [`index.html`](file:///c:/Users/USER/Desktop/projects/dis-app/ui-design/index.html) in your browser. It includes:
1. **Interactive Mobile App Simulator**: Switch between Auth, Server Hub, Chat, Voice Call, and In-Game Floating HUD.
2. **Interactive State Simulator**: Test speaker VAD animation pulses, Wi-Fi ↔ LTE handoff banners, realtime incoming chat messages, and audio peripheral routing.
3. **Jetpack Compose Component Catalog**: Modular component contracts, props, and design tokens.

---

## Jetpack Compose Component Hierarchy

```
app/
├── ui/
│   ├── theme/
│   │   ├── Color.kt              # AMOLED #0B0C10, Indigo #4F46E5, Emerald #10B981
│   │   ├── Type.kt               # High-contrast gamer typography
│   │   └── Theme.kt
│   ├── auth/
│   │   ├── AuthScreen.kt         # Onboarding with avatar picker & handle input
│   │   └── AvatarPicker.kt       # Emoji/icon selector
│   ├── navigation/
│   │   ├── ServerNavigationRail.kt # Vertical server icon list
│   │   └── BottomUserDock.kt     # Self status + quick mute toggle
│   ├── channels/
│   │   ├── ChannelListScreen.kt  # Text & Voice channel list
│   │   ├── VoiceChannelTile.kt   # Live attendee count & speaker indicator
│   │   └── TextChannelTile.kt    # Unread counter badge
│   ├── chat/
│   │   ├── ChatScreen.kt         # Inverted LazyColumn with Room cache paging
│   │   ├── MessageBubble.kt      # Message card with timestamp & avatar
│   │   ├── ReconnectBanner.kt    # Sync indicator (resuming by last_message_id)
│   │   └── ChatInputDock.kt      # Sticky message field + send button
│   ├── voice/
│   │   ├── VoiceRoomScreen.kt    # LiveKit WebRTC call view
│   │   ├── SpeakerGridTile.kt    # Dynamic pulsing green aura on active speaker
│   │   ├── VoiceControlsBar.kt   # Mute, Deafen, BT Route, Disconnect
│   │   └── AudioRoutePicker.kt   # Bluetooth / Speaker / Earpiece switch
│   └── overlay/
│       ├── FloatingOverlayService.kt # Android SYSTEM_ALERT_WINDOW floating HUD
│       ├── FloatingMuteBubble.kt # Draggable one-tap mic toggle over games
│       └── ActiveSpeakerToast.kt # Minimalist speaker pill
```

---

## Core Feature Mappings

| Feature | Android UI / Service Component | Backend Contract |
| :--- | :--- | :--- |
| **Frictionless Auth** | `AuthScreen.kt` | `auth.proto` (`SignUp`, `Login`) |
| **Channels & Live Badges** | `ChannelListScreen.kt` | `groups.proto` (`ListChannels`, `ListParticipants`) |
| **Instant Text Chat** | `ChatScreen.kt` + `Room Paging 3` | `chat.proto` (`SyncMessages`, `StreamMessages`) |
| **In-Game Voice WebRTC** | `VoiceRoomScreen.kt` + `ForegroundService` | `voice.proto` (`GetVoiceToken` -> LiveKit) |
| **In-Game Overlay Mute** | `FloatingOverlayService.kt` | Local state synced with LiveKit audio track |
