# MeetMeAt - Conference Poster Generator

Create branded "I'm attending" posters for social media. Select profile, enter event URL, AI generates templates, customize, and export.

## Repositories

This project consists of 3 separate Git repositories:

| Repo | Description |
|------|-------------|
| [meetmeat-docs](https://github.com/AlexTheWizardL/meetmeat-docs) | Documentation, designs, specs (this repo) |
| [meetmeat-backend](https://github.com/AlexTheWizardL/meetmeat-backend) | NestJS API |
| [meetmeat-frontend](https://github.com/AlexTheWizardL/meetmeat-frontend) | React Native + Expo app |

---

## Quick Start

### Prerequisites

- Node.js 20+
- Docker (for PostgreSQL)
- npm

### 1. Clone All Repositories

```bash
# Clone docs (includes nested repo folders)
git clone https://github.com/AlexTheWizardL/meetmeat-docs.git
cd meetmeat-docs

# Clone backend into docs folder
git clone https://github.com/AlexTheWizardL/meetmeat-backend.git

# Clone frontend into docs folder
git clone https://github.com/AlexTheWizardL/meetmeat-frontend.git
```

### 2. Start Backend

```bash
cd meetmeat-backend

# Install dependencies
npm install

# Configure environment
cp .env.example .env
# Edit .env with your API keys (OpenAI/Anthropic)

# Start PostgreSQL via Docker
docker compose up -d

# Run backend in development mode
npm run start:dev
```

Backend runs at: http://localhost:3000
API docs: http://localhost:3000/api/docs

### 3. Start Frontend

```bash
cd meetmeat-frontend

# Install dependencies (legacy-peer-deps for React 19 compatibility)
npm install --legacy-peer-deps

# Start web development
npm run web
```

Frontend runs at: http://localhost:8081

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React Native + Expo SDK 54 + React Native Web |
| Graphics | React Native Skia |
| Backend | NestJS 11 + TypeORM + PostgreSQL |
| AI | OpenAI / Anthropic (swappable via env) |
| Storage | Local / S3 (swappable via env) |

---

## Project Structure

```
meetmeat-docs/
├── CLAUDE.md            # AI assistant instructions
├── PROJECT_NOTES.md     # Tech decisions & progress
├── TECHNICAL_SPEC.md    # Data models & API spec
├── UX_UI_APPROACH.md    # Screen flows & UX logic
├── designs/screens/     # UI designs (10 screens)
├── archive/             # Research documents
├── meetmeat-backend/    # NestJS API (separate git repo)
└── meetmeat-frontend/   # Expo app (separate git repo)
```

---

## Implementation Status

### Completed

- [x] **UI Designs** - 10 screens designed
- [x] **Data Models** - TECHNICAL_SPEC.md
- [x] **Backend API** - NestJS with 98 unit tests + 15 e2e tests
- [x] **AI Providers** - OpenAI (full), Anthropic (mock)
- [x] **Storage Providers** - Local, S3
- [x] **Docker Setup** - PostgreSQL compose
- [x] **Frontend Screens** - 8/10 implemented:
  - Home, Your Details, Event Details, Loading
  - Editor, Export, Profiles, Profile Form
- [x] **API Client** - Axios with hooks
- [x] **State Management** - Zustand store

### Pending (MVP Blockers)

- [ ] **History Screen** - Currently shows placeholder, needs real data
- [ ] **Poster Detail Screen** - Not implemented
- [ ] **Integration Testing** - Frontend to backend

### Future Enhancements

- [ ] Complete Anthropic provider (currently returns mock data)
- [ ] Implement GCS storage provider
- [ ] Real poster image rendering (currently placeholder SVG)
- [ ] Add rate limiting to API
- [ ] Add test coverage for storage providers

---

## Available Scripts

### Backend (meetmeat-backend/)

```bash
npm run start:dev   # Development with watch mode
npm run build       # Production build
npm run test        # Unit tests (98 tests)
npm run test:e2e    # End-to-end tests (15 tests)
npm run lint        # ESLint
```

### Frontend (meetmeat-frontend/)

```bash
npm run web         # Start web development
npm run ios         # Start iOS simulator
npm run android     # Start Android emulator
npm run lint        # ESLint (strict)
npm run typecheck   # TypeScript check
npm run format      # Prettier
```

---

## Environment Variables

### Backend (.env)

```bash
# Database
DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_USER=meetmeat
DATABASE_PASSWORD=your_password
DATABASE_NAME=meetmeat_dev

# AI Provider (openai or anthropic)
AI_PROVIDER=openai
OPENAI_API_KEY=sk-...
# or
# AI_PROVIDER=anthropic
# ANTHROPIC_API_KEY=sk-ant-...

# Storage (local or s3)
STORAGE_PROVIDER=local
STORAGE_LOCAL_PATH=./uploads
# For S3:
# AWS_REGION=us-east-1
# AWS_ACCESS_KEY_ID=...
# AWS_SECRET_ACCESS_KEY=...
# S3_BUCKET_NAME=...
```

---

## API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/profiles` | GET, POST | Manage user profiles |
| `/profiles/:id` | GET, PATCH, DELETE | Single profile operations |
| `/events/parse` | POST | Parse event from URL using AI |
| `/templates/generate` | POST | Generate poster templates |
| `/posters` | GET, POST | Manage posters |
| `/posters/:id/export` | POST | Export for social media |

Full API documentation available at `/api/docs` when backend is running.

---

## Design System

```
Colors:
  Primary:    #6C5CE7 (purple)
  Background: #FFFFFF
  Input BG:   #F5F5F7
  Text:       #1A1A2E
  Muted:      #8E8E93
  Danger:     #E74C3C

Spacing: 8, 12, 16, 20, 24px
Corners: 12px (cards), 8px (inputs)
```

---

## Screen Designs

| # | Screen | Design File |
|---|--------|-------------|
| 1 | Home | `designs/screens/01-home.png` |
| 2 | Your Details | `designs/screens/02-your-details.png` |
| 3 | Event Details | `designs/screens/03-event-details.png` |
| 4 | Loading | `designs/screens/04-loading.png` |
| 5 | Editor | `designs/screens/05-editor.png` |
| 6 | Export | `designs/screens/06-export.png` |
| 7 | History | `designs/screens/07-history.png` |
| 8 | Profiles | `designs/screens/08-profiles.png` |
| 9 | Profile Form | `designs/screens/09-profile-form.png` |
| 10 | Poster Detail | `designs/screens/10-poster-detail.png` |

---

## License

MIT
