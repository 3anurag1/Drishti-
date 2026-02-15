# NetraPay (MVP)

**Eyes you trust. Payments you control.**

NetraPay is a voice-first UPI payment assistant for blind users, with **human-in-the-loop approval** by a trusted sighted guardian.

This repo contains:
- `mobile/`: Expo React Native app (Blind + Guardian roles)
- `backend/`: Node.js + Express API (Firebase Admin + Firestore + FCM)

## What’s implemented (MVP)

- **Phone auth (OTP)** via Firebase Auth (client-side)
- **Role selection** on first login (blind / guardian)
- **Secure pairing**
  - Blind user generates a **6-digit pairing code**
  - Guardian enters code → backend permanently links both accounts
- **Blind flow**
  - Home auto-opens camera
  - Continuous QR detection (UPI QR)
  - Voice command parsing: “Pay 250” / “Pay 250 rupees”
  - Captures a QR image
  - Creates a **payment request** to paired guardian (expires in 2 minutes)
  - Audio confirmations (TTS)
- **Guardian flow**
  - Receives FCM push notification for new request
  - Views request (QR image + decoded UPI + amount)
  - Approve → opens UPI app via deep link (GPay/PhonePe/etc.)
  - Reject → updates status

## Tech choices

- **Mobile**: Expo (React Native), Firebase client SDK, `expo-camera`, `expo-speech`, `expo-notifications`
- **Backend**: Express + Firebase Admin (Firestore + FCM)
- **QR parsing**: parses UPI URI from QR payload, extracts `pa` / `pn`
- **Speech-to-text (STT)**: MVP uses **device STT** via a dev-client compatible module (see `mobile/README_STT.md`)
  - Expo doesn’t include robust always-on STT out of the box; we ship a production-viable path using a custom dev client.

## Quick start

### 1) Firebase project

Create a Firebase project with:
- Authentication: **Phone**
- Firestore
- Storage
- Cloud Messaging (FCM)

Download:
- Android `google-services.json` → place at `mobile/google-services.json`
- A service account JSON → place at `backend/serviceAccount.json` (DO NOT COMMIT)

### 2) Backend

```bash
cd backend
npm install
cp .env.example .env
npm run dev
```

### 3) Mobile

```bash
cd mobile
npm install
cp .env.example .env
npx expo start
```

## Environment variables

- Backend: `backend/.env.example`
- Mobile: `mobile/.env.example`

## Notes (important)

- **NetraPay never moves money**. It only creates a verified request and deep-links into the user’s chosen UPI app.
- **Requests expire after 2 minutes** and are single-use.
- **Only the paired guardian** can approve a request.

