# MeetMeAt - Project Instructions

> This file is automatically read by Claude Code at session start.
> **No need to paste anything** - Claude Code reads this file automatically when working in this folder.

## User Context

**User:** Web developer, medium React knowledge, learning React Native & mobile development.

**Claude should:**
- Explain mobile-specific concepts (build process, signing, native modules, etc.)
- Highlight differences from web development
- Provide context on "why" not just "how"
- Point out critical steps that are easy to miss
- Offer to explain any technical decisions in more detail

## Project Overview

**MeetMeAt** - Conference poster generator app. Users create branded "I'm attending" posters for social media.

**Flow:** Select profile → Enter event URL → AI generates templates → Customize → Export for LinkedIn/Instagram/Twitter/Facebook

## Tech Stack

- **Frontend:** React Native + Expo + React Native Web
- **Editor:** React Native Skia (swappable architecture)
- **Backend:** NestJS + TypeORM + PostgreSQL
- **AI:** OpenAI/Claude (swappable via env)

---

## File References

> **Read these files when needed for detailed info:**

| File | Contains | Read When |
|------|----------|-----------|
| `PROJECT_NOTES.md` | Tech decisions, MVP scope, progress checklist | Starting work, checking what's done |
| `UX_UI_APPROACH.md` | Screen flows, state management, UX decisions | Implementing screens, understanding navigation |
| `designs/screens/XX-name.png` | Visual UI designs | Implementing any screen (see list below) |
| `archive/canvas-editor-research.md` | Editor library research, Skia examples | Working on poster editor |
| `archive/DESIGN_PROMPTS.md` | AI prompts for generating UI designs | Creating new screen designs |
| `archive/RESEARCH.md` | React Native vs Flutter comparison | Reference only |

---

## Screen → Design Mapping

| Screen | Design File | Read design before implementing |
|--------|-------------|--------------------------------|
| Home | `designs/screens/01-home.png` | Profile selector, URL input, recent posters |
| Your Details | `designs/screens/02-your-details.png` | Step 1 form |
| Event Details | `designs/screens/03-event-details.png` | Step 2 form, brand colors |
| Loading | `designs/screens/04-loading.png` | AI generation animation |
| Editor | `designs/screens/05-editor.png` | Poster preview, templates, customize |
| Export | `designs/screens/06-export.png` | Platform selection |
| History | `designs/screens/07-history.png` | Poster list by month |
| Profiles | `designs/screens/08-profiles.png` | Saved profiles list |
| Profile Form | `designs/screens/09-profile-form.png` | Add/edit profile modal |
| Poster Detail | `designs/screens/10-poster-detail.png` | View + actions |

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

## Repositories & Git

### Repo URLs
| Repo | GitHub URL | Local Path |
|------|------------|------------|
| meetmeat-docs | https://github.com/AlexTheWizardL/meetmeat-docs | `./` (root) |
| meetmeat-backend | https://github.com/AlexTheWizardL/meetmeat-backend | `./meetmeat-backend/` |
| meetmeat-frontend | https://github.com/AlexTheWizardL/meetmeat-frontend | `./meetmeat-frontend/` |

### Git Commands (Claude Reference)

**Docs repo (root):**
```bash
cd "/Users/oleksandrstepanenko/unibrix/mobile apps/mobile-app-research"
git add . && git commit -m "message" && git push
```

**Backend repo:**
```bash
cd "/Users/oleksandrstepanenko/unibrix/mobile apps/mobile-app-research/meetmeat-backend"
git add . && git commit -m "message" && git push
```

**Frontend repo:**
```bash
cd "/Users/oleksandrstepanenko/unibrix/mobile apps/mobile-app-research/meetmeat-frontend"
git add . && git commit -m "message" && git push
```

### Initialize Backend/Frontend (First Time)
```bash
# Backend
cd meetmeat-backend
git init
git remote add origin https://github.com/AlexTheWizardL/meetmeat-backend.git

# Frontend
cd meetmeat-frontend
git init
git remote add origin https://github.com/AlexTheWizardL/meetmeat-frontend.git
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

- [x] Designs complete (10 screens)
- [ ] Data models (TECHNICAL_SPEC.md)
- [ ] API endpoints
- [ ] Frontend setup
- [ ] Backend setup

---

## Repetitive Tasks (Claude Should Handle)

### After Making Changes
1. Update `PROJECT_NOTES.md` progress section if milestone completed
2. Update this file's "Current Status" if major progress

### After Significant Work Session
Offer to commit:
```bash
git add .
git commit -m "Description of changes"
git push
```

### When Implementing a Screen
1. Read the design PNG first: `designs/screens/XX-name.png`
2. Reference `UX_UI_APPROACH.md` for interactions/state
3. Use design system colors/spacing from this file

### When Creating New Files
- Frontend components: `meetmeat-frontend/src/`
- Backend modules: `meetmeat-backend/src/`
- New docs: root folder, update README.md

### When User Asks "What's Next?"
1. Check `PROJECT_NOTES.md` progress section
2. Suggest next unchecked item
3. Read relevant reference files before starting
