# The Lo

The Lo is a campus event map for the University of Minnesota. It answers one question: **what's happening near me right now?**

Users drop pins on a live map to signal events — parties, study groups, food trucks, club meetings, pop-ups. Everyone nearby sees them in real time. Tap a pin to see details, like it, get directions, or attach a photo.

## Why

UMN has 50,000+ students and no single place to find spontaneous, real-time campus events. Instagram stories disappear. GroupMe threads get buried. Flyers don't scale. The Lo is a shared map that makes the campus feel smaller.

## Scope (MVP)

What we're building first:

- Drop event markers on a map with a name, category, and description
- See all active markers on the map in real time
- Like/unlike events
- Attach photos to events
- Search for venues by name
- Sign up / log in

What we're NOT building yet:

- Push notifications
- Event scheduling (future dates)
- Comments / chat
- User profiles
- Admin moderation panel

## Tech

```
frontend/   React Native (Expo SDK 55) + TypeScript
backend/    Go + PostgreSQL + PostGIS
```

---

## Getting Started

### Prerequisites

| Tool | Version | Check |
|------|---------|-------|
| Node.js | 22+ | `node -v` |
| npm | 10+ | `npm -v` |
| Go | 1.22+ | `go version` |
| PostgreSQL | 16+ with PostGIS | `psql --version` |
| Xcode | Latest (for iOS sim) | `xcode-select -v` |

### 1. Clone the repo

```bash
git clone https://github.com/Mulla759/the-lo-2.git
cd the-lo-2
```

### 2. Set up the backend

```bash
cd backend
cp .env.example .env
```

Open `backend/.env` and fill in:

```
DATABASE_URL=postgres://your_user:your_password@localhost:5432/thelo_dev?sslmode=disable
JWT_SECRET=any-random-string-at-least-32-chars
GOOGLE_PLACES_API_KEY=your-google-api-key
PORT=8080
```

Create the database:

```bash
createdb thelo_dev
```

Run the server:

```bash
go run ./cmd/server
```

Verify it's alive:

```bash
curl http://localhost:8080/health
```

You should see `{"status":"ok"}`.

### 3. Set up the frontend

Open a second terminal:

```bash
cd frontend
npm install
cp config/env/.env.example config/env/.env
```

Open `frontend/config/env/.env` and fill in:

```
EXPO_PUBLIC_GOOGLE_MAPS_API_KEY=your-google-maps-js-api-key
EXPO_PUBLIC_API_URL=http://localhost:8080
```

If running on a physical device instead of the simulator, replace `localhost` with your computer's local IP:

```
EXPO_PUBLIC_API_URL=http://192.168.x.x:8080
```

Find your IP with `ipconfig getifaddr en0`.

### 4. Run the app

```bash
npm run ios -- --tunnel
```

Other options:

```bash
npm run start       # Expo dev server only
npm run android     # Android emulator
npm run web         # Web browser
```

### 5. Verify everything works

```bash
npm run typecheck   # TypeScript
npm run lint        # ESLint
npm test            # Jest
npx expo-doctor     # Expo health check
```

---

## Project Layout

```
the-lo-2/
├── .github/workflows/
│   ├── frontend-ci.yml      runs on frontend/ changes
│   ├── backend-ci.yml       runs on backend/ changes
│   └── codeql.yml           security scanning (TS + Go)
├── frontend/
│   ├── src/
│   │   ├── app/             App.tsx, AppRoot.tsx, registerRoot.ts
│   │   └── screens/         HomeScreen.tsx, SignInScreen.tsx
│   ├── config/env/          .env.example
│   ├── assets/              app icons, splash screen
│   ├── docs/                CODE_STANDARDS.md
│   ├── package.json
│   └── tsconfig.json
├── backend/
│   ├── cmd/server/          main.go (entry point)
│   ├── internal/            handlers, models, middleware, db
│   ├── migrations/          SQL migration files
│   ├── Dockerfile
│   ├── go.mod
│   └── .env.example
└── README.md
```

---

## CI

GitHub Actions runs automatically on every PR:

- **Frontend CI** — typecheck, lint, test, expo-doctor, Expo tunnel smoke test (only triggers when `frontend/` changes)
- **Backend CI** — go build, go vet, staticcheck, go test with race detector against a PostGIS container (only triggers when `backend/` changes)
- **CodeQL** — security scanning for TypeScript and Go

---

## Team

3 people. See `frontend/docs/CODE_STANDARDS.md` for coding standards and review expectations.