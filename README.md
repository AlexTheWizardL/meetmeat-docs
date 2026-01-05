# MeetMeAt

Conference poster generator. Create branded "I'm attending" posters for social media.

**Flow:** Select profile → Enter event URL → AI generates templates → Customize → Export

## Quick Start

### Prerequisites

- Node.js 20+
- Docker (for PostgreSQL)

### 1. Start Backend

```bash
cd meetmeat-backend
npm install

# Create env file
cp .env.example .env.local
# Edit .env.local: add OPENAI_API_KEY

# Start PostgreSQL only
docker compose up -d postgres

# Start API
npm run start:dev
```

- API: http://localhost:3000
- Swagger: http://localhost:3000/api/docs

### 2. Start Frontend

```bash
cd meetmeat-frontend
npm install --legacy-peer-deps
npm run web
```

Frontend: http://localhost:8081

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React Native + Expo SDK 54 |
| Graphics | React Native Skia |
| Backend | NestJS 11 + TypeORM + PostgreSQL |
| AI | OpenAI (GPT-4 Vision + DALL-E 3) |

## Project Structure

```
meet-me-at/
├── meetmeat-backend/     # NestJS API
├── meetmeat-frontend/    # Expo app
├── designs/screens/      # UI designs
└── docs/                 # UX documentation
```

## Status

**Done:** Backend API (99 tests), Frontend (8/10 screens)

**Needs work:**
- AI backgrounds don't always match site brand styles
- Template elements placement is random/odd
- Frontend has no customization yet (can't move/edit elements)
- History screen, Poster Detail screen not implemented

## Scripts

### Backend

```bash
npm run start:dev   # Development
npm run test        # Tests (99)
npm run build       # Build
```

### Frontend

```bash
npm run web         # Web
npm run ios         # iOS
npm run android     # Android
```

## License

MIT
