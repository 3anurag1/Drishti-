# Speech-to-text (STT) for NetraPay

NetraPay’s blind flow is designed to be **voice-only**. Expo (managed) does not currently provide a robust always-on STT module.

## Production-ready options

### Option A (recommended): Expo dev client + native Android SpeechRecognizer

- Add a small native module (Kotlin) exposing Android `SpeechRecognizer`.
- Build an Expo dev client (`npx expo prebuild` + `expo run:android`).
- Implement `startListeningOnce()` in `src/voice/stt.ts`.

Pros: on-device, fast, low latency, no cloud cost.  
Cons: needs dev client (not Expo Go).

### Option B: Record audio and send to backend STT

- Record with `expo-av`
- Send to backend endpoint (Google Speech-to-Text / Whisper)
- Return recognized transcript

Pros: consistent accuracy across devices.  
Cons: network cost + privacy considerations.

## Current MVP behavior

`startListeningOnce()` currently returns `null` so the app runs end-to-end without STT wiring.
You can still validate:
- QR scan
- request creation
- guardian approval + UPI deep link

