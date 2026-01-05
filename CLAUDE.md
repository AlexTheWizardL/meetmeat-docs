# MeetMeAt - Project Instructions

> This file is automatically read by Claude Code at session start.

## Project Overview

**MeetMeAt** - Conference poster generator app. Users create branded "I'm attending" posters for social media.

**Flow:** Select profile → Enter event URL → AI generates templates → Customize → Export for LinkedIn/Instagram/Twitter/Facebook

## Tech Stack

- **Frontend:** React Native + Expo SDK 54 + React Native Web
- **Editor:** React Native Skia (swappable architecture)
- **Backend:** NestJS 11 + TypeORM + PostgreSQL
- **AI:** OpenAI/Anthropic (swappable via env)
- **Storage:** Local/S3 (swappable via env)

---

## Repository Structure

**3 separate git repos** (NOT submodules):

| Repo | GitHub URL | Local Path |
|------|------------|------------|
| meetmeat-docs | github.com/AlexTheWizardL/meetmeat-docs | `./` (root) |
| meetmeat-backend | github.com/AlexTheWizardL/meetmeat-backend | `./meetmeat-backend/` |
| meetmeat-frontend | github.com/AlexTheWizardL/meetmeat-frontend | `./meetmeat-frontend/` |

**Important:** Backend and frontend folders are nested inside docs folder BUT have their own `.git` directories. They are independent repos, not submodules.

---

## File References

| File | Contains |
|------|----------|
| `PROJECT_NOTES.md` | Tech decisions, MVP scope, progress checklist |
| `UX_UI_APPROACH.md` | Screen flows, state management, UX decisions |
| `TECHNICAL_SPEC.md` | Data models, API specification |
| `designs/screens/` | UI designs (10 screens) |
| `archive/` | Research files (editor, framework comparison) |

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
Corners: 12px (cards), 8px (inputs), 50% (avatars)
```

---

## MVP Constraints

- No auth (local storage only)
- Basic editing (text/color, no drag-drop)
- Native share sheet (not direct API posting)
- Templates always switchable
- AI provider swappable via env

---

## Current Status

**Completed:**
- [x] UI designs (10 screens)
- [x] Data models & API spec (TECHNICAL_SPEC.md)
- [x] Backend API (NestJS + 98 unit tests + 15 e2e tests)
- [x] Swappable providers (AI: OpenAI/Anthropic, Storage: Local/S3)
- [x] Docker setup for PostgreSQL
- [x] Frontend screens (9/10 implemented)
- [x] API client layer + React hooks
- [x] Zustand store for poster creation flow

**Remaining:**
- [ ] Poster Detail screen (10/10)
- [ ] Integration testing (frontend ↔ backend)
- [ ] MVP release

---

## Git Commit Rules

- Use standard format: `<type>: <description>`
- Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`
- Keep commits atomic and descriptive
