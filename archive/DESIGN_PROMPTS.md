# MeetMeAt - AI Design Prompts

## Style Reference (From Home Screen)

```
DESIGN SYSTEM:
- Primary color: Purple/violet (#6C5CE7)
- Background: White (#FFFFFF)
- Input fields: Light gray (#F5F5F7)
- Text: Dark (#1A1A2E)
- Secondary text: Gray (#8E8E93)
- Corners: 12px radius on cards/buttons/inputs
- Shadows: Subtle, light gray
- Icons: Outlined, simple line style
- Typography: Sans-serif, bold headers, medium body
- Bottom tab bar: Home (house), History (clock), Profiles (person)
- FAB: Purple circle with + icon (bottom right)
```

---

## Screen 1: Home ✅ COMPLETED

Reference for all other screens.

---

## Screen 2: Your Details (Step 1 of 2)

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT (Previous screen - Home):
- User tapped "Enter Details Manually" button
- App has purple (#6C5CE7) primary color
- Clean white background with light gray input fields
- Rounded corners (12px), subtle shadows
- Bottom tab bar with Home, History, Profiles
- Professional, minimal style

SCREEN: Your Details Form (Step 1 of 2)

Layout from top to bottom:

1. HEADER ROW
   - Back arrow (←) on left (gray, tappable)
   - "Step 1 of 2" on right (gray text, small)

2. TITLE
   - "Your Details" (large, bold, left-aligned)
   - Subtitle: "Fill in your information" (gray, smaller)

3. AVATAR SECTION (centered)
   - Circular photo placeholder (80px diameter)
   - Default: gray background with person silhouette
   - "Change Photo" text button below (purple text)

4. FORM FIELDS (full width, stacked, 16px gap)
   Each field has: light gray background, left icon (gray), placeholder text

   - Email (envelope icon) - "your@email.com"
   - Full Name (person icon) - "Your full name"
   - Position (briefcase icon) - "Your job title"
   - Company Name (building icon) - "Company name"

5. COMPANY LOGO BUTTON
   - Outlined button (gray border, not filled)
   - Upload icon + "Upload Company Logo"
   - "(Optional)" text in gray

6. SAVE PROFILE CHECKBOX
   - Unchecked checkbox + "Save as new profile"
   - Gray text

7. NEXT BUTTON (bottom, above tab bar)
   - Full width purple button
   - "Next" text + arrow icon (→)
   - Same style as "Generate Poster" on Home

8. BOTTOM TAB BAR
   - Same as Home screen
   - Home tab should be active (user came from Home)

Match the exact input field style from Home screen (the "Paste event URL..." field).
Keep generous padding and clean visual hierarchy.
```

---

## Screen 3: Event Details (Step 2 of 2)

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT (Previous screens):
- Screen 1 (Home): Purple primary, white bg, light gray inputs, bottom tabs
- Screen 2 (Your Details): User filled personal info, form field style established
- User tapped "Next" to get here
- Same design system continues

SCREEN: Event Details Form (Step 2 of 2)

Layout from top to bottom:

1. HEADER ROW
   - Back arrow (←) on left
   - "Step 2 of 2" on right

2. TITLE
   - "Event Details" (large, bold)
   - Subtitle: "Tell us about the conference" (gray)

3. FORM FIELDS (same style as Step 1)
   - Event Name (calendar icon) - "Conference name"
   - Event Dates (calendar icon) - "Nov 10-13, 2025"
   - Location (map pin icon) - "City, Country"
   - Event Website (link icon) - "https://..." (optional)

4. EVENT LOGO BUTTON
   - Same style as company logo button from Step 1
   - Upload icon + "Upload Event Logo"
   - "(Optional)" text

5. BRAND COLORS SECTION
   - Label: "Brand Colors" with "(Optional)" gray text
   - Subtitle: "We'll try to detect from website"
   - Two color picker circles side by side:
     - "Primary" label + color circle (tappable)
     - "Secondary" label + color circle (tappable)
   - Default: gray placeholder circles

6. GENERATE BUTTON (prominent, bottom)
   - Full width purple button
   - Sparkle icon (✨) + "Generate Poster"
   - Same style as Home screen button

7. BOTTOM TAB BAR
   - Same as previous screens
   - Home still active

Show a subtle progress indicator or step dots if possible.
Maintain exact same spacing and field styles as Step 1.
```

---

## Screen 4: Loading/Generating State

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT:
- User tapped "Generate Poster" on Event Details screen
- AI is analyzing the event and generating templates
- Same purple (#6C5CE7) design system
- This is a transitional loading state

SCREEN: Generating Poster (Loading State)

Layout (centered, minimal):

1. NO HEADER (full screen experience)

2. CENTER CONTENT (vertically and horizontally centered)
   - Animated sparkle/magic icon or circular progress
   - Large, prominent, purple color

3. TEXT BELOW ANIMATION
   - "Creating your poster..." (large, bold)
   - Rotating status messages (gray, smaller):
     - "Analyzing event branding..."
     - "Extracting colors and style..."
     - "Generating templates..."

4. SUBTLE PROGRESS BAR (optional)
   - Thin purple line at top
   - Animated, indeterminate progress

5. NO BOTTOM TAB BAR (full focus on loading)

Keep it clean and focused. The animation should feel magical/AI-powered.
Purple gradient background is acceptable here for emphasis.
```

---

## Screen 5: Editor with Template Selection

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT (Previous screens):
- Home: Profile selection, URL input, purple primary color
- Form screens: Established input/button styles
- Loading: AI generated templates
- User now sees their poster options
- Same design system: purple, white bg, 12px corners

SCREEN: Poster Editor with Template Switching

This is the MAIN editing screen. Key feature: templates are always accessible.

Layout from top to bottom:

1. HEADER ROW
   - Back arrow (←) on left
   - "Edit Poster" title center
   - "Export" text button on right (purple)

2. POSTER PREVIEW (60% of screen height)
   - Large card with subtle shadow
   - Shows the actual poster design:
     - Event logo/branding at top
     - User photo (from profile)
     - "Meet with [Name]" text
     - Position + Company
     - Event name, dates, location
     - Company logo at bottom
   - Subtle overlay hint: "Tap to edit elements"

3. TEMPLATE SELECTOR (horizontal scroll)
   - Label: "Templates" on left, "3 styles" on right (gray)
   - 3 template thumbnails in horizontal scroll:
     - Small cards (70x90px) with poster preview
     - Active template: purple border (2px)
     - Inactive: gray border
     - Tap to instantly switch

4. EDIT PANEL (contextual, bottom sheet style)
   - Appears when element is selected
   - Light gray background, rounded top corners
   - Shows controls based on selected element:

   For TEXT elements:
   - Text input field with current text
   - Font size slider (S to L)
   - Color options: 5 circles from event palette

   For IMAGE elements:
   - "Change Photo" button
   - Fit options: Fill / Fit / Crop icons

   Default state (nothing selected):
   - "Tap any element to edit" hint text
   - Color palette preview from event

5. NO BOTTOM TAB BAR (editor is full-screen mode)

The poster preview should look realistic - like the Web Summit example.
Template thumbnails should show actual different layouts.
```

---

## Screen 6: Export Screen

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT:
- User tapped "Export" from Editor screen
- They've customized their poster
- Now choosing platform and export format
- Same design system continues

SCREEN: Export Poster

Layout from top to bottom:

1. HEADER ROW
   - Back arrow (←) on left
   - "Export" title center

2. POSTER PREVIEW (centered, medium size)
   - Shows final poster design
   - Card with shadow
   - Aspect ratio adjusts based on selected platform

3. PLATFORM SELECTION
   - Label: "Select Platform" (bold)
   - 2x2 grid of platform cards:

   Each card shows:
   - Platform icon (official logo style)
   - Platform name
   - Optimal size text (gray, small)
   - Selected state: purple border + checkmark

   Cards:
   - LinkedIn (blue icon) - "1200 × 627"
   - Instagram (gradient icon) - "1080 × 1080"
   - Twitter/X (black icon) - "1200 × 675"
   - Facebook (blue icon) - "1200 × 630"

4. SIZE PREVIEW
   - Small text showing: "Preview adjusted to LinkedIn size"
   - Updates when platform changes

5. ACTION BUTTONS (bottom)
   - Two buttons side by side:
   - "Download" (outlined, gray border, download icon)
   - "Share" (filled purple, share icon)
   - Equal width, 12px gap between

6. NO BOTTOM TAB BAR (export flow is modal)

Platform cards should be clearly tappable with visual feedback.
LinkedIn should be pre-selected as default.
```

---

## Screen 7: History Tab

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT:
- User tapped "History" in bottom tab bar
- Shows all previously created posters
- Same design system: purple primary, white bg
- This is a main tab, not a modal

SCREEN: History Tab

Layout from top to bottom:

1. HEADER
   - "History" title (large, bold, left)
   - "Edit" text button on right (purple) - for delete mode

2. SEARCH/FILTER (optional)
   - Search bar: "Search posters..." (light gray bg)
   - Or filter chips: "All", "LinkedIn", "Instagram", etc.

3. POSTER LIST (grouped by month)

   Section header: "December 2025" (gray, uppercase, small)

   Poster cards (list style):
   - Thumbnail on left (50x65px, rounded corners)
   - Content on right:
     - Event name (bold) - "Web Summit 2025"
     - Person name (regular) - "Sarah Miller"
     - Date + platform icon - "Dec 8 · LinkedIn"
   - Chevron (>) on far right
   - Card has white bg, subtle shadow, full width

   Show 3-4 example entries across 2 months

4. EMPTY STATE (if no history)
   - Centered illustration (simple, line art style)
   - "No posters yet" (bold)
   - "Create your first conference poster" (gray)
   - "Create Poster" button (purple, outlined)

5. BOTTOM TAB BAR
   - History tab active (filled icon, purple)
   - Home and Profiles inactive (gray outlines)

6. FAB BUTTON
   - Same purple + circle as Home screen
   - Quick access to create new poster

Tapping a poster card opens it for viewing/re-editing.
```

---

## Screen 8: Profiles Tab

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT:
- User tapped "Profiles" in bottom tab bar
- Shows saved user profiles for quick poster creation
- Same design system throughout
- Profiles were referenced in Home screen dropdown

SCREEN: Profiles Tab

Layout from top to bottom:

1. HEADER
   - "Profiles" title (large, bold, left)
   - "+ Add" text button on right (purple)

2. PROFILE CARDS (list)

   Each profile card shows:
   - Avatar on left (50px circle)
   - Content:
     - Name (bold) - "Sarah Miller"
     - Position + Company - "Product Designer @ TechFlow"
     - Email (gray, smaller) - "sarah@techflow.com"
   - "Edit" text button on right (gray)
   - Card: white bg, subtle shadow, rounded corners

   Show 2-3 example profiles

3. ADD PROFILE CARD (at bottom of list)
   - Dashed border (gray)
   - Centered content:
     - Plus icon in circle (gray)
     - "Add New Profile" text
   - Full width, same height as profile cards
   - Tappable

4. EMPTY STATE (if no profiles)
   - Person illustration (simple)
   - "No profiles saved" (bold)
   - "Save your details for faster poster creation" (gray)
   - "Add Profile" button (purple)

5. BOTTOM TAB BAR
   - Profiles tab active (filled, purple)
   - Home and History inactive (gray)

Tapping "Edit" or a profile card opens edit form.
Tapping "+ Add" or the add card opens new profile form.
```

---

## Screen 9: Profile Edit/Add Form

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT:
- User tapped "+ Add" or "Edit" on Profiles tab
- Reuses form style from "Your Details" screen
- Same input fields, same styling
- Can be used for both add and edit modes

SCREEN: Add/Edit Profile

Layout from top to bottom:

1. HEADER ROW
   - Close (X) icon on left (gray)
   - "New Profile" or "Edit Profile" title center
   - "Save" text button on right (purple, disabled until valid)

2. AVATAR SECTION (centered)
   - Large circular avatar (100px)
   - Camera icon overlay on bottom right
   - "Change Photo" text below (purple)

3. FORM FIELDS (same style as Your Details)
   - Full Name (person icon)
   - Email (envelope icon)
   - Position (briefcase icon)
   - Company Name (building icon)

4. COMPANY LOGO
   - Upload button (outlined)
   - If uploaded: shows thumbnail with X to remove

5. DELETE BUTTON (only in edit mode)
   - At very bottom
   - Red text: "Delete Profile"
   - No background, just text button

6. NO BOTTOM TAB BAR (this is a modal/sheet)

Form validation:
- Name and Email are required
- Save button activates when required fields filled
- Show inline error states if needed (red border, error text)

This should feel like a modal sheet overlaying the Profiles tab.
```

---

## Screen 10: Poster Detail View (from History)

```
Design a mobile app screen for "MeetMeAt" - a professional poster generator app.

CONTEXT:
- User tapped a poster from History tab
- Shows full poster with action options
- Can re-export, edit, duplicate, or delete

SCREEN: Poster Detail View

Layout from top to bottom:

1. HEADER ROW
   - Back arrow (←) on left
   - "Poster Details" center
   - More menu (•••) on right

2. POSTER PREVIEW (large, centered)
   - Full poster display
   - Card with shadow
   - Takes most of the screen

3. METADATA (below poster)
   - Event name (bold)
   - Created date: "December 8, 2025"
   - Profile used: "Sarah Miller"
   - Last exported: "LinkedIn • 1200×627"

4. ACTION BUTTONS (horizontal row)
   - Icon buttons with labels below:
   - Edit (pencil icon) - "Edit"
   - Export (download icon) - "Export"
   - Duplicate (copy icon) - "Duplicate"
   - Share (share icon) - "Share"
   - All gray icons, purple on tap

5. DELETE OPTION
   - At bottom, red text: "Delete Poster"
   - Or in the more menu (•••)

6. NO BOTTOM TAB BAR (detail view is modal)

Tapping "Edit" goes to Editor screen with this poster loaded.
Tapping "Export" goes to Export screen.
Tapping "Duplicate" creates copy and goes to Editor.
```

---

## Summary: Screen Generation Order

| # | Screen | Status |
|---|--------|--------|
| 1 | Home | ✅ Done |
| 2 | Your Details (Step 1) | Next → |
| 3 | Event Details (Step 2) | Pending |
| 4 | Loading State | Pending |
| 5 | Editor + Templates | Pending |
| 6 | Export | Pending |
| 7 | History Tab | Pending |
| 8 | Profiles Tab | Pending |
| 9 | Profile Add/Edit | Pending |
| 10 | Poster Detail | Pending |

---

## Tips for Consistency

1. Always reference "same as Home screen" for colors, corners, shadows
2. Keep input field style identical across all forms
3. Button styles: filled purple (primary), outlined gray (secondary)
4. Use same icon weight/style throughout
5. Maintain consistent spacing (16px standard gap)
6. Tab bar only on main tabs (Home, History, Profiles)
7. No tab bar on modals/flows (Editor, Export, Forms)
