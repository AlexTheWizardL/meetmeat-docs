# MeetMeAt - Project Notes

## Overview

App that creates branded conference posters. User selects profile, enters event URL, AI generates templates, user customizes and exports for social media.

## Tech Stack

| Layer | Technology |
|-------|------------|
| Frontend | React Native + Expo + React Native Web |
| Editor | React Native Skia (swappable) |
| Backend | NestJS + TypeORM + PostgreSQL |
| AI | OpenAI / Claude (swappable) |

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
| Swappable AI provider | Env variable switches OpenAI/Claude |
| Swappable editor | Can swap Skia/Fabric/Polotno |
| Templates always switchable | User can change anytime, edits preserved |
| Basic editing only | Text/color edits, no drag-drop for MVP |
| Native share sheet | Not direct API posting |

## MVP Features

**Included:**
- Profile management (CRUD)
- Event URL → AI analysis
- Manual event entry (fallback)
- 2-3 AI-generated templates
- Basic editing (text, colors)
- Export to 4 platforms
- History with re-edit

**Not in MVP:**
- Drag & drop
- Rotation/resize
- Custom fonts
- Direct social posting
- Auth/cloud sync

## Progress

- [x] Concept defined
- [x] UI designed (10 screens)
- [x] Editor research done
- [x] Data models (TECHNICAL_SPEC.md)
- [x] API endpoints (TECHNICAL_SPEC.md)
- [x] Backend setup (NestJS + TypeORM + 113 tests)
- [x] Frontend setup (Expo SDK 54 + Skia + strict ESLint)
- [ ] Connect frontend to backend
- [ ] MVP complete

## Export Sizes

| Platform | Size |
|----------|------|
| LinkedIn | 1200×627 |
| Instagram | 1080×1080 |
| Twitter/X | 1200×675 |
| Facebook | 1200×630 |

---
*Last Updated: December 15, 2025*
