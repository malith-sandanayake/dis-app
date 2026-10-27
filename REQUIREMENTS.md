# Project Requirements: Gaming Voice & Chat App (MVP)

## 1. Overview & Core Differentiator
The application is a lightweight, low-latency communication platform tailored specifically for mobile gamers, featuring:
- **Go Backend** with Protobuf definitions.
- **Android Client** optimized for in-game performance.
- **LiveKit** integration for real-time WebRTC audio.
- **Core Differentiator:** Background voice communication that stays alive, maintains clear audio without ducking/muting game sound, uses minimal battery/CPU, seamlessly reconnects across Wi-Fi/cellular transitions, and provides in-game controls without leaving the game.

---

## 2. MVP Scope

### 2.1 Accounts (Minimum Viable)
- **Sign Up & Log In**: Simple credential-based authentication (token-based session management).
- **Profile**: Username and avatar selection.
- **Out of Scope for MVP**: Email verification, passkeys, phone verification, and 2FA.

### 2.2 Groups & Channels
- **Group Management**:
  - Create a group.
  - Join a group via invite code or link.
- **Channels**:
  - Text channels.
  - Voice channels.
- **Roles & Permissions**:
  - Basic roles: `Owner`, `Admin`, `Member`.
- **Out of Scope for MVP**: Categories, fine-grained custom permissions, bots, threads.

### 2.3 Text Chat
- **Real-time Messaging**: Instant message exchange over persistent connection (WebSocket / gRPC streaming).
- **Local Cache & Pagination**: Chats load instantly from local storage (e.g., Room / SQLite); backward pagination on scroll.
- **Reconnect & Resumption**: On reconnection, fetch undelivered messages resuming from the last acknowledged message ID.
- **Push Notifications**: Firebase Cloud Messaging (FCM) integration for background message delivery.
- **Out of Scope for MVP**: Message reactions, file/media uploads, full-text search, message editing, message deletion.

### 2.4 Voice During Games (Critical Focus)
- **Call Controls**:
  - Join and leave voice channels.
  - Self-mute and self-deafen.
  - Audio output routing (Speaker, Earpiece, Bluetooth headset).
- **Channel State**:
  - Display active participants in the voice channel.
  - Active speaker indication (speaking indicator highlighting).
- **Input Modes**:
  - Voice Activity Detection (VAD).
  - Push-to-Talk (PTT).
- **Out of Scope for MVP**: Video calls, screen sharing, music/radio bots.

---

## 3. In-Game Voice Architecture (Android Technical Requirements)

This is the make-or-break differentiator on Android:

1. **Foreground Service & Persistent Notification**:
   - Run voice WebRTC sessions inside an Android `ForegroundService` with a ongoing notification (`FOREGROUND_SERVICE_TYPE_MICROPHONE`).
   - Prevent the Android OS from killing the audio pipeline when heavy 3D games occupy the foreground.

2. **Audio Focus Handling & Game Audio Coexistence**:
   - Configure Android audio attributes for voice communication (`STREAM_VOICE_CALL` / `USAGE_VOICE_COMMUNICATION`).
   - Manage `AudioManager` audio focus requests so game audio and voice chat play simultaneously without game audio cutting out or being overly ducked.

3. **Low CPU & Battery Consumption**:
   - Audio codec: Opus configured for speech at modest bitrates (~20–32 kbps).
   - Enable Discontinuous Transmission (**DTX**) to stop sending packets during silence.
   - Built-in hardware/software Acoustic Echo Cancellation (AEC) and Noise Suppression (NS).

4. **Resilient Network Switching**:
   - Smoothly survive mid-game Wi-Fi to cellular (and vice versa) handoffs with rapid ICE restart / LiveKit reconnection without dropping the call.

5. **Floating Overlay Mute Button**:
   - Floating widget overlay using `SYSTEM_ALERT_WINDOW` permission to allow muting/unmuting directly while gaming, without switching apps. *(To be built once core voice engine is stable).*

6. **Target Hardware Validation**:
   - Continuous testing and profiling on real mid-range physical Android devices under active gaming load (not just emulators).

---

## 4. Phased Build Order

```
[Phase 1: Proto & Backend Skeleton]
  └── Protobuf schemas, Go backend (auth, groups, channels, token generation)
[Phase 2: Android Core UI & Auth]
  └── Login/signup screens, group/channel list, navigation
[Phase 3: Realtime Text Chat]
  └── WebSocket/gRPC streaming, Room local cache, resumption by last message ID
[Phase 4: Push Notifications]
  └── FCM integration for offline/background text alerts
[Phase 5: In-Game Voice (LiveKit)]
  └── LiveKit integration, Android ForegroundService, audio focus management, VAD/PTT
[Phase 6: Floating Overlay & In-Game Testing]
  └── SYSTEM_ALERT_WINDOW floating mute button, benchmarks on mid-range devices during games
```

> **Rule of Thumb:** Steps 1 to 4 provide a solid, testable text platform. Do not commence voice engine integration until text chat state synchronization is robust.
