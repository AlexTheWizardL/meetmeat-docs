# MeetMeAt - Project Instructions

> Auto-read by Claude Code at session start.

## Overview

Conference poster generator. Users create branded "I'm attending" posters for social media.

**Flow:** Select profile → Enter event URL → AI generates templates → Customize → Export

## Tech Stack

- **Frontend:** React Native + Expo SDK 54 + React Native Web
- **Editor:** React Native Skia
- **Backend:** NestJS 11 + TypeORM + PostgreSQL
- **AI:** OpenAI (GPT-4 Vision + DALL-E 3)
- **Storage:** Local / S3

## Project Structure

```
meet-me-at/
├── meetmeat-backend/     # NestJS API
├── meetmeat-frontend/    # Expo app
├── designs/              # UI designs
└── docs/                 # UX documentation
```

## Key Files

| File | Contains |
|------|----------|
| `docs/UX_UI_APPROACH.md` | Screen flows, UX decisions |
| `designs/screens/` | UI designs (10 screens) |

## Design System

```
Colors:
  Primary:    #6C5CE7
  Background: #FFFFFF
  Input BG:   #F5F5F7
  Text:       #1A1A2E
  Muted:      #8E8E93

Spacing: 8, 12, 16, 20, 24px
Corners: 12px (cards), 8px (inputs)
```

## Status

**Done:** UI designs, Backend (99 tests), AI integration, Frontend (8/10 screens)

**Pending:** History screen, Poster Detail screen, Integration testing

## Git

- Format: `<type>: <description>`
- Types: `feat`, `fix`, `docs`, `refactor`, `test`, `chore`
