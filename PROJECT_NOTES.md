# MeetMeAt - Project Notes

## Overview

App that creates branded conference posters. User selects profile, enters event URL, AI generates templates, user customizes and exports for social media.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React Native + Expo SDK 54 + React Native Web |
| Graphics | React Native Skia |
| Backend | NestJS 11 + TypeORM + PostgreSQL |
| AI | OpenAI / Anthropic (swappable via env) |
| Storage | Local / S3 (swappable via env) |

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

## Key Decisions

| Decision | Rationale |
|----------|-----------|
| No auth for MVP | Local storage only, simplify |
| Swappable AI provider | Env variable switches OpenAI/Anthropic |
| Swappable storage | Env variable switches Local/S3 |
| Templates always switchable | User can change anytime, edits preserved |
| Basic editing only | Text/color edits, no drag-drop for MVP |
| Native share sheet | Not direct API posting |

## Progress

### Completed

- [x] UI designed (10 screens)
- [x] Data models (TECHNICAL_SPEC.md)
- [x] Backend API (NestJS + TypeORM)
- [x] 98 unit tests + 15 e2e tests
- [x] Swappable AI providers (OpenAI full, Anthropic mock)
- [x] Swappable storage (Local, S3)
- [x] Docker setup for PostgreSQL
- [x] Frontend setup (Expo SDK 54 + Skia)
- [x] Frontend screens (8/10):
  - Home screen
  - Your Details (Step 1)
  - Event Details (Step 2)
  - Loading screen
  - Editor screen
  - Export screen
  - Profiles screen
  - Profile Form modal
- [x] API client layer (lib/api/)
- [x] React hooks (useProfiles, usePosters, useEvents, useTemplates)
- [x] Zustand store for poster creation flow

### Pending (MVP Blockers)

- [ ] History screen (currently placeholder)
- [ ] Poster Detail screen (not implemented)
- [ ] Integration testing (frontend ↔ backend)

### Future Enhancements

- [ ] Complete Anthropic provider (currently mock)
- [ ] Implement GCS storage provider
- [ ] Real poster image rendering (currently placeholder)
- [ ] Rate limiting for API
- [ ] Additional test coverage

## Export Sizes

| Platform | Size |
|----------|------|
| LinkedIn | 1200×627 |
| Instagram | 1080×1080 |
| Twitter/X | 1200×675 |
| Facebook | 1200×630 |

---
*Last Updated: January 2026*
