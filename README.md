# NetraPay Backend

Express API backed by Firebase Admin (Firestore + FCM).

## Setup

1. Place a Firebase service account at `backend/serviceAccount.json`.
2. Create `.env` from example:

```bash
cp .env.example .env
```

3. Install + run:

```bash
npm install
npm run dev
```

## Auth

All `/api/*` routes require:

`Authorization: Bearer <Firebase ID token>`

The mobile app obtains this ID token from Firebase Auth (Phone OTP).

## Routes (summary)

- `GET /api/health`
- `POST /api/me` (role + fcmToken)
- `POST /api/pairing/code` (blind only)
- `POST /api/pairing/redeem` (guardian only)
- `POST /api/requests` (blind only)
- `GET /api/requests` (guardian only)
- `PATCH /api/requests/:id` (guardian only; approve/reject)

