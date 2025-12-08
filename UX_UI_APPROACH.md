# MeetMeAt - UI/UX Approach

## Design References

> **Design files location:** `designs/screens/`

| Screen | Design File | Description |
|--------|-------------|-------------|
| Home | ![](designs/screens/01-home.png) | `01-home.png` - Main screen with profile selector, URL input, recent posters |
| Your Details | ![](designs/screens/02-your-details.png) | `02-your-details.png` - Step 1: Personal info form |
| Event Details | ![](designs/screens/03-event-details.png) | `03-event-details.png` - Step 2: Event info + brand colors |
| Loading | ![](designs/screens/04-loading.png) | `04-loading.png` - AI generation in progress |
| Editor | ![](designs/screens/05-editor.png) | `05-editor.png` - Poster preview + template switching + customize |
| Export | ![](designs/screens/06-export.png) | `06-export.png` - Platform selection + download/share |
| History | ![](designs/screens/07-history.png) | `07-history.png` - List of created posters by month |
| Profiles | ![](designs/screens/08-profiles.png) | `08-profiles.png` - Saved profiles list |
| Profile Form | ![](designs/screens/09-profile-form.png) | `09-profile-form.png` - Add/edit profile modal |
| Poster Detail | ![](designs/screens/10-poster-detail.png) | `10-poster-detail.png` - View poster with actions |

---

## Core UX Principles

1. **Minimal friction** - Saved profiles, auto-fill, smart defaults
2. **Non-destructive editing** - Switch templates anytime, nothing is lost
3. **Progressive complexity** - Simple MVP, advanced features later
4. **History matters** - Access all previous creations easily

---

## Navigation Structure

```
Tab Bar (Bottom)
├── Home (Create)
├── History
├── Profiles
└── Settings (later)
```

---

## Screen Flow

### 1. HOME (Create New)

```
┌─────────────────────────────────────┐
│  MeetMeAt                           │
├─────────────────────────────────────┤
│                                     │
│  Quick Start                        │
│  ┌─────────────────────────────┐   │
│  │ Select Profile      [▼]     │   │
│  │ ● Valera (CEO)              │   │
│  │ ○ Alex (CTO)                │   │
│  │ ○ + New Profile             │   │
│  └─────────────────────────────┘   │
│                                     │
│  Event                              │
│  ┌─────────────────────────────┐   │
│  │ Paste event URL...          │   │
│  └─────────────────────────────┘   │
│                                     │
│  [ Generate Poster ]                │
│                                     │
│  ─── or ───                         │
│                                     │
│  [ Enter Details Manually ]         │
│                                     │
├─────────────────────────────────────┤
│  Recent                             │
│  ┌────┐ ┌────┐ ┌────┐              │
│  │Web │ │AWS │ │... │  → see all   │
│  │Sum │ │re: │ │    │              │
│  └────┘ └────┘ └────┘              │
└─────────────────────────────────────┘
```

**Key interactions:**
- Select saved profile → auto-fills all personal data
- Paste URL → "Generate Poster" becomes active
- "Enter Details Manually" → goes to manual form

---

### 2. MANUAL FORM (Optional Path)

```
┌─────────────────────────────────────┐
│  ← Back         Step 1/2            │
├─────────────────────────────────────┤
│                                     │
│  Your Details                       │
│                                     │
│  ┌──────────┐                       │
│  │  Photo   │  [Change]             │
│  └──────────┘                       │
│                                     │
│  Email         [________________]   │
│  Name          [________________]   │
│  Position      [________________]   │
│  Company       [________________]   │
│  Company Logo  [Upload] (optional)  │
│                                     │
│  [ ] Save as new profile            │
│                                     │
│              [ Next → ]             │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│  ← Back         Step 2/2            │
├─────────────────────────────────────┤
│                                     │
│  Event Details                      │
│                                     │
│  Event Name    [________________]   │
│  Dates         [________________]   │
│  Location      [________________]   │
│  Event Logo    [Upload] (optional)  │
│                                     │
│  Brand Colors (optional)            │
│  Primary   [■]  Secondary [■]       │
│                                     │
│         [ Generate Poster ]         │
└─────────────────────────────────────┘
```

---

### 3. TEMPLATE SELECTION + EDITOR (Combined View)

**Key UX Change:** Templates are always accessible, not a one-time choice.

```
┌─────────────────────────────────────┐
│  ← Back              [Export]       │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐   │
│  │                             │   │
│  │                             │   │
│  │     LIVE POSTER PREVIEW     │   │
│  │     (Selected Template)     │   │
│  │                             │   │
│  │     Tap element to edit     │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  Templates (swipe to switch)        │
│  ┌────┐ ┌────┐ ┌────┐              │
│  │ ●  │ │ ○  │ │ ○  │   [+] More   │
│  └────┘ └────┘ └────┘              │
│                                     │
│  ─── Edit Selected Element ───      │
│  (appears when element tapped)      │
│                                     │
│  Text: [__________________]         │
│  Color: [■][■][■][■][+]            │
│  Size:  [- ████████████ +]         │
│                                     │
└─────────────────────────────────────┘
```

**Key interactions:**
- Tap template thumbnail → instantly switches preview
- All user data transfers between templates automatically
- Tap element on preview → shows edit panel for that element
- Edit panel is contextual (text vs image vs shape)

**MVP Edit Capabilities:**
- Text: edit content, color, size
- Image: swap, basic resize
- Colors: change from event palette + custom

**Later (not MVP):**
- Drag to reposition
- Pinch to resize
- Rotation
- Layer ordering

---

### 4. EXPORT

```
┌─────────────────────────────────────┐
│  ← Back           Export            │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐   │
│  │                             │   │
│  │    Final Preview            │   │
│  │    (with selected size)     │   │
│  │                             │   │
│  └─────────────────────────────┘   │
│                                     │
│  Platform                           │
│  ┌────────┐ ┌────────┐             │
│  │LinkedIn│ │  Insta │             │
│  │  ●     │ │   ○    │             │
│  │1200x627│ │1080x   │             │
│  └────────┘ └────────┘             │
│  ┌────────┐ ┌────────┐             │
│  │Twitter │ │Facebook│             │
│  │   ○    │ │   ○    │             │
│  └────────┘ └────────┘             │
│                                     │
│  [ Download ]  [ Share... ]         │
│                                     │
└─────────────────────────────────────┘
```

**Post-export:** Auto-saves to History

---

### 5. HISTORY TAB

```
┌─────────────────────────────────────┐
│  History                    [Edit]  │
├─────────────────────────────────────┤
│                                     │
│  December 2025                      │
│  ┌─────────────────────────────┐   │
│  │ ┌────┐  Web Summit 2025     │   │
│  │ │    │  Valera Olexienko    │   │
│  │ └────┘  Dec 8 · LinkedIn    │   │
│  └─────────────────────────────┘   │
│  ┌─────────────────────────────┐   │
│  │ ┌────┐  AWS re:Invent       │   │
│  │ │    │  Alex Stepanenko     │   │
│  │ └────┘  Dec 5 · Instagram   │   │
│  └─────────────────────────────┘   │
│                                     │
│  November 2025                      │
│  ┌─────────────────────────────┐   │
│  │ ┌────┐  React Conf 2025     │   │
│  │ │    │  Valera Olexienko    │   │
│  │ └────┘  Nov 20 · Twitter    │   │
│  └─────────────────────────────┘   │
│                                     │
└─────────────────────────────────────┘
```

**Tap item actions:**
- View full poster
- Re-edit (opens editor with all data)
- Export again (different platform)
- Duplicate (use as base for new)
- Delete

---

### 6. PROFILES TAB

```
┌─────────────────────────────────────┐
│  Profiles                [+ Add]    │
├─────────────────────────────────────┤
│                                     │
│  ┌─────────────────────────────┐   │
│  │ ┌────┐  Valera Olexienko    │   │
│  │ │foto│  CEO · Unibrix       │   │
│  │ └────┘  valera@unibrix.com  │   │
│  │                      [Edit] │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │ ┌────┐  Alex Stepanenko     │   │
│  │ │foto│  CTO · Unibrix       │   │
│  │ └────┘  alex@unibrix.com    │   │
│  │                      [Edit] │   │
│  └─────────────────────────────┘   │
│                                     │
│  ┌─────────────────────────────┐   │
│  │         + Add Profile        │   │
│  └─────────────────────────────┘   │
│                                     │
└─────────────────────────────────────┘
```

**Profile contains:**
- Photo (fetched from email or uploaded)
- Full name
- Position/Title
- Company name
- Company logo (optional)
- Email

---

## State Management Concept

```
AppState
├── profiles[]          ← Saved user profiles
├── currentPoster       ← Active editing session
│   ├── profileId       ← Which profile is used
│   ├── event           ← Scraped/manual event data
│   ├── templates[]     ← AI-generated templates
│   ├── activeTemplate  ← Currently selected index
│   └── customizations  ← User edits (per template)
└── history[]           ← Completed posters
```

**Key insight:** `customizations` is stored per-template, so switching templates preserves edits.

---

## Architecture: Swappable Editor

```
┌─────────────────────────────────────┐
│           EditorProvider            │
│  (abstract interface)               │
├─────────────────────────────────────┤
│  - renderCanvas()                   │
│  - selectElement(id)                │
│  - updateElement(id, props)         │
│  - exportToImage(format, size)      │
└─────────────────────────────────────┘
          │
          ├── SkiaEditorProvider (default)
          │   └── uses @shopify/react-native-skia
          │
          ├── FabricEditorProvider (future web-only)
          │   └── uses fabric.js via WebView
          │
          └── PolotnoEditorProvider (fallback)
              └── uses Polotno SDK
```

**Config:**
```typescript
// config.ts
export const EDITOR_PROVIDER = process.env.EDITOR_PROVIDER || 'skia';

// EditorContext.tsx
const providers = {
  skia: SkiaEditorProvider,
  fabric: FabricEditorProvider,
  polotno: PolotnoEditorProvider,
};

export const EditorProvider = providers[EDITOR_PROVIDER];
```

---

## MVP Scope Summary

### Included in MVP
- [x] Profile management (CRUD)
- [x] Event URL → AI analysis
- [x] Manual event entry
- [x] 2-3 AI-generated templates
- [x] Switch between templates freely
- [x] Basic editing (text, colors)
- [x] Export to 4 platforms
- [x] History list
- [x] Re-edit from history

### NOT in MVP (Later)
- [ ] Drag & drop repositioning
- [ ] Pinch to resize
- [ ] Rotation
- [ ] Custom fonts
- [ ] Stickers/graphics library
- [ ] Direct social media posting (just share sheet)
- [ ] Team/organization features
- [ ] Cloud sync

---

## User Flows

### Flow 1: Quick Create (Happy Path)
```
Home → Select Profile → Paste URL → Generate →
Pick Template → Quick Edit → Export → Done (saved to History)
```
**Time:** ~1 minute

### Flow 2: New User
```
Home → Enter Manually → Fill Form (check "Save Profile") →
Enter Event → Generate → Pick Template → Export → Done
```
**Time:** ~3 minutes (first time), then Flow 1

### Flow 3: Re-export from History
```
History → Tap Item → Export Again → Pick Different Platform → Done
```
**Time:** ~20 seconds

### Flow 4: Duplicate & Modify
```
History → Tap Item → Duplicate → Change Event URL → Generate → Export
```
**Time:** ~1 minute

---

## Key UX Decisions

| Decision | Rationale |
|----------|-----------|
| Templates always visible | No commitment anxiety, encourages exploration |
| Profiles as first-class feature | Reduces friction for repeat use |
| Combined template + editor view | Less navigation, faster iteration |
| Auto-save to history | Never lose work |
| Platform selection at export | Different needs per post |
| MVP: limited editing | Ship faster, add features based on feedback |
