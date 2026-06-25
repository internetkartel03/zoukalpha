# ZOUK Desktop Application — Visual & Technical Reference for Claude Code

**This document is the approved visual reference for rebuilding ZOUK as a clean React + TypeScript + Vite project.**

---

## 📋 Project Overview

**ZOUK** is a production-ready AI workstation desktop application. The current export is a Design Component prototype with working UI and state management. Your task is to convert it into a clean, scalable React + TypeScript + Vite codebase while **preserving the exact visual design**.

**DO NOT redesign the UI.** Use this document as your visual anchor.

---

## 🎨 Design System

### Colors

| Name | Value | Usage |
|------|-------|-------|
| **Black (Primary BG)** | `#000000` | Page background, overlays |
| **Dark Gray (Sidebar)** | `#0a0a0a` | Sidebar, panels, cards |
| **Dark Gray 2** | `#060606` | Content areas, screen backgrounds |
| **Dark Gray 3** | `#1a1a1a` | Borders, dividers, subtle surfaces |
| **Dark Gray 4** | `#2a2a2a` | Hover states, secondary borders |
| **Red (Accent)** | `#ec1d2e` | Buttons, highlights, CTAs, glows |
| **Red (Hover/Bright)** | `#ff5664` | Hover states on red elements |
| **Text Primary** | `#e8e8e8` | Main text, headings |
| **Text Secondary** | `#b5b5b5` | Secondary text, labels |
| **Text Tertiary** | `#9a9a9a` | Tertiary text, descriptions |
| **Text Muted** | `#6a6a6a` | Placeholders, disabled states |
| **Green (Success)** | `#3ed47f` | Connected status indicators |

### Typography

- **Font Family**: `Inter` (primary), `-apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif` (fallback)
- **Font Stack in CSS**: `font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;`
- **Monospace (for code/prompts)**: `monospace`

**Font Sizes & Weights:**
- H1 (page titles): `28px font-weight: 700`
- H2 (section titles): `28px font-weight: 700`
- H3 (subsection): `16px font-weight: 600`
- Label/Button: `14px font-weight: 600` or `500`
- Body text: `14px font-weight: 400`
- Small text: `13px font-weight: 400`
- Tiny text: `12px font-weight: 400` or `500`
- Extra tiny: `11px font-weight: 400`

### Spacing Scale

- `4px` – minimal gaps
- `8px` – tight spacing
- `12px` – default padding/gap
- `16px` – medium spacing
- `20px` – larger gaps
- `24px` – section spacing
- `28px` – large section spacing
- `32px` – page padding

### Border Radius

- Buttons, inputs: `6px`, `8px`
- Cards, panels: `12px`
- Large modals, hero areas: `18px`
- Avatars, badge circles: `50%`

### Shadows & Glows

**Box Shadows (Red Accent Glow):**
```css
/* Subtle glow */
box-shadow: 0 0 16px rgba(236,29,46,0.4);

/* Medium glow */
box-shadow: 0 0 40px rgba(236,29,46,0.12), inset 0 0 30px rgba(0,0,0,0.5);

/* Strong breathing glow */
box-shadow: 
  0 0 55px rgba(236,29,46,0.35), 
  inset 0 0 65px rgba(236,29,46,0.2);
```

**Text Shadows:**
```css
text-shadow: 0 2px 26px rgba(0,0,0,0.6);
```

### Animations & Transitions

```css
/* Standard transition */
transition: all .15s;

/* Fade in on entry */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(8px); }
  to { opacity: 1; transform: translateY(0); }
}
animation: fadeIn .3s ease-out;

/* Breathing glow (red accent) */
@keyframes breathe {
  0%, 100% { 
    box-shadow: 0 0 55px rgba(236,29,46,0.35), 
                inset 0 0 65px rgba(236,29,46,0.2); 
  }
  50% { 
    box-shadow: 0 0 90px rgba(236,29,46,0.6), 
                inset 0 0 80px rgba(236,29,46,0.32); 
  }
}

/* Shimmer effect */
@keyframes shimmer {
  0% { background-position: -180% 0; }
  100% { background-position: 280% 0; }
}
```

---

## 🏗️ Layout Architecture

### Main Container

```
<div style="display: flex; width: 100%; height: 100vh; background: #000;">
  <!-- Sidebar (left) -->
  <!-- Main Content (right) -->
</div>
```

**Key Properties:**
- `display: flex` (row layout)
- `width: 100%`, `height: 100vh`
- `background: #000`
- `position: absolute` on child sections for routing

### Sidebar

**Width:** `248px` (expanded), `64px` (collapsed)  
**Transition:** `width 0.3s ease`

**Structure:**
1. **Logo & Toggle** – 64px height, centered, with chevron toggle
2. **New Chat Button** – Red outline style, full width
3. **Nav Menu** – Flex column, scrollable, icon + text
4. **Settings** – Fixed at bottom, above user card
5. **User Card** – Avatar + name/plan, collapsed hides text

**Nav Items:**
- New Chat
- Skills / Agents
- Builder (App Builder)
- Library
- Projects → New Project
- All Tasks
- Connectors
- Settings

**Sidebar Styling:**
```css
width: 248px;
background: #0a0a0a;
border-right: 1px solid #1a1a1a;
display: flex;
flex-direction: column;
transition: width 0.3s ease;
overflow: hidden;
```

### Main Content Area

**Structure:**
- Full viewport minus sidebar
- `position: absolute; left: {{ sidebarWidth }}; right: 0; top: 0; bottom: 0`
- Background image + overlay (dark scrim)
- Route-based sections inside

**Background:**
```css
background-image: url('assets/bg-hero.png');
background-size: cover;
background-position: center center;
```

**Overlay Scrim:**
```css
background: linear-gradient(180deg, rgba(0,0,0,0.05) 0%, rgba(0,0,0,0.2) 100%);
pointer-events: none;
```

---

## 📱 Screen Sections (Routes)

### 1. Home Screen (`section === 'home'`)

**Layout:**
- Centered content, bottom-aligned in viewport
- Hero height: `414px`
- Composer width: `max-width: 860px`

**Elements (top to bottom):**

1. **Main Heading**
   ```
   "What can I [build] for you?"
   Font: 40px, 600, #f4f4f4
   Color accent on "build": #ec1d2e
   ```

2. **Prompt Composer (Large Hero)**
   ```
   Border: 1px solid rgba(236,29,46,0.45)
   Border-radius: 18px
   Background: linear-gradient(180deg, #0b0809, #060606)
   Box-shadow: 0 0 40px rgba(236,29,46,0.12), inset 0 0 30px rgba(0,0,0,0.5)
   Padding: 22px 24px 6px
   
   Contains:
   - Textarea (flex: 1, min-height: 56px, max-height: 160px)
   - Spark icon (right side)
   ```

3. **Composer Controls (bottom)**
   ```
   Padding: 8px 18px 16px
   Display: flex, space-between
   
   Left side:
   - Plus button (38px circle, add menu)
   - Meta Ads icon (38px blue, #1877f2)
   - Google Ads icon (38px gradient)
   - YouTube Ads icon (38px white)
   - More button (38px dark)
   
   Right side:
   - Prompt settings button
   - Send button (46px circle, red border glow)
   ```

4. **Action Chips**
   ```
   Margin-top: 28px
   Flex row, wrap, center
   
   Three buttons:
   - Create Ads (🎯)
   - Build Website (🌐)
   - Automate Workflow (⚙️)
   
   Style: background: #0b0b0b, border: 1px solid #1c1c1c
   Hover: border-color: #ec1d2e
   ```

### 2. Skills / Agents Screen (`section === 'skills-agents'`)

**Layout:**
- Full-screen content, scrollable
- Max-width: 1400px, centered, 32px padding

**Sections:**
1. **Header** – H2 title, description paragraph
2. **Filter & Action Buttons**
   - All / Skills / Agents (toggle buttons)
   - Create Skill
   - Create Agent
   - Upload .md

3. **Grid** – `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))`

**Card Style:**
```css
background: #0a0a0a;
border: 1px solid #1a1a1a;
border-radius: 12px;
padding: 16px;
```

### 3. Builder Workshop (`section === 'builder-workshop'`)

**Three-Panel Layout:**
```
Top Bar (fixed)
│
├─ Left Panel (320px) ── Center Panel (flex) ── Right Panel (300px)
│  Chat Messages       Canvas Preview       Tweaks/Assets
```

**Top Bar:**
- Background: `#0a0a0a`, border-bottom: `1px solid #1a1a1a`
- Left: Back button + project name + "Live Preview" label
- Right: AI Model selector + Publish menu

**Left Chat Panel (320px):**
- Scrollable message area
- Image/File/Component buttons at bottom
- Textarea + Send button

**Center Canvas:**
- Background gradient: `linear-gradient(135deg, #0a0a0a 0%, #060606 100%)`
- Phone mockup (portrait, centered)
- Mockup size: 352px width, 860px height (scaled down)
- Screen content: 251px × 508px inner display

**Right Tweaks Panel (300px):**
- Two tabs: Tweaks | Assets
- Tab buttons with bottom border indicator
- Tweaks: Title input, Description textarea, Color pickers
- Assets: List of uploaded files

### 4. Library Screen (`section === 'library'`)

**Layout:**
- Full-screen, scrollable
- Max-width: 1400px

**Grid:**
- `grid-template-columns: repeat(auto-fill, minmax(200px, 1fr))`
- Cards: 200px wide, with icon area (120px height)

### 5. Connectors Screen (`section === 'connectors'`)

**Layout:**
- Full-screen, scrollable
- Grouped by category (Advertising, AI, etc.)

**Category Section:**
- H3 title
- Grid: `repeat(auto-fill, minmax(280px, 1fr))`

**Connector Card:**
```css
background: #0a0a0a;
border: 1px solid #1a1a1a;
border-radius: 12px;
padding: 16px;

Contains:
- Icon + name + category
- Description text
- Connect/Connected button
```

### 6. Projects Screen (`section === 'projects'`)

**Layout:**
- Full-screen, scrollable
- Grid: `repeat(auto-fill, minmax(300px, 1fr))`

**Card:**
- Name (heading), Description, Open button

### 7. Tasks Screen (`section === 'tasks'`)

**Layout:**
- Full-screen, scrollable
- Flex column, gap: 12px

**Task Row:**
- Name (left), Project + time label (left), Continue button (right)

### 8. Settings Screen (`section === 'settings'`)

**Layout:**
- Full-screen, scrollable
- Max-width: 900px

**Tabs:**
- Profile
- User Flows
- AI Models
- Workspace

**Profile Tab:**
- Photo upload area (80px avatar)
- Full Name input
- Email input
- Change Password section
- Save button

**User Flows Tab:**
- User flow cards (grid)
- New User Flow button

**AI Models Tab:**
- Default Model dropdown
- Temperature slider
- Max tokens input
- System Prompt textarea
- Safety checkboxes
- Save button

**Workspace Tab:**
- Feature toggles
- Connector shortcut
- Danger Zone (Reset)

---

## 🧩 Component Patterns

### Buttons

**Primary Red Button:**
```css
background: rgba(236,29,46,0.12);
border: 1px solid #ec1d2e;
border-radius: 8px;
color: #ec1d2e;
font-weight: 600;
cursor: pointer;
transition: all .15s;

:hover {
  background: rgba(236,29,46,0.2);
}
```

**Dark Button:**
```css
background: #1a1a1a;
border: 1px solid #2a2a2a;
border-radius: 8px;
color: #b5b5b5;
cursor: pointer;
transition: all .15s;

:hover {
  border-color: #ec1d2e;
  color: #ec1d2e;
}
```

**Circle Button (38-46px):**
```css
width: 38px;
height: 38px;
border-radius: 50%;
display: flex;
align-items: center;
justify-content: center;
cursor: pointer;
```

### Inputs & Textareas

```css
background: #0a0a0a;
border: 1px solid #1c1c1c;
border-radius: 8px;
color: #e8e8e8;
font-size: 14px;
padding: 10px 12px;

::placeholder {
  color: #6a6a6a;
}

:focus {
  outline: none;
  border-color: #ec1d2e;
}
```

### Cards

```css
background: #0a0a0a;
border: 1px solid #1a1a1a;
border-radius: 12px;
padding: 16px;
animation: fadeIn .3s ease-out;

:hover {
  border-color: #ec1d2e;
}
```

### Scrollbars

```css
::-webkit-scrollbar { width: 8px; height: 8px; }
::-webkit-scrollbar-thumb { background: #2a2a2a; border-radius: 4px; }
::-webkit-scrollbar-track { background: transparent; }
```

---

## 🎯 Assets & Files

**Location:** `assets/`

| File | Purpose | Dimensions |
|------|---------|-----------|
| `zouk-logo.png` | Sidebar logo, onboarding | ~80×40px |
| `bg-hero.png` | Main background image | Full screen |
| `bg-loop.mp4` | Background video (hero area) | Looped |
| `phone-mockup-portrait.png` | Builder canvas mockup | 352×860px |

**Uploads Folder:** User-uploaded images, generated assets, screenshots

---

## 📊 Data Structures

### Skills/Agents (`.md` format)

**Skill Template:**
```yaml
---
type: skill
name: Ad Campaign Builder
category: Marketing
description: Builds ad campaign strategy...
version: 1.0
---
[Markdown body with instructions]
```

**Agent Template:**
```yaml
---
type: agent
name: Growth Operator
role: Marketing execution agent
description: Turns business goals into campaigns...
tools:
  - Meta Ads
  - Google Ads
version: 1.0
---
[Markdown body with behavior instructions]
```

### Connectors

```ts
interface Connector {
  id: string;
  name: string;
  category: 'Advertising' | 'AI' | 'Automation' | 'Development' | 'Commerce' | 'Communication' | 'Storage' | 'Productivity';
  description: string;
  icon?: string;
  iconBg?: string;
  glyph?: string;
  status: 'Not Connected' | 'Connected' | 'API Key Needed' | 'Login Required';
  connectionType: 'oauth' | 'api_key' | 'manual';
  connected: boolean;
  connectedAt?: number;
  services?: string[]; // for umbrella connectors like Meta Ads
}
```

**Default Connectors Array** (minimal):
- Meta Ads (advertising)
- Google Ads (advertising)
- YouTube Ads (advertising)
- Anthropic Claude (AI)
- OpenAI (AI)
- GitHub (development)
- Stripe (commerce)
- Slack (communication)

### Projects & Tasks

```ts
interface Project {
  id: string;
  name: string;
  description: string;
  createdAt: number;
  updatedAt: number;
  tasks: Task[];
  attachedAssets: string[];
  connectedTools: string[];
}

interface Task {
  id: string;
  projectId: string;
  title: string;
  messages: Message[];
  selectedSkill?: string;
  selectedAgent?: string;
  attachedAssets: string[];
  connectedTools: string[];
  createdAt: number;
  updatedAt: number;
}

interface Message {
  id: string;
  role: 'user' | 'assistant';
  text: string;
  timestamp: number;
}
```

---

## 💾 State & localStorage

**Key:** `zouk-state`

**Persisted Fields:**
- `sidebarOpen`
- `userName`, `userPhoto`, `userEmail`, `userInitial`
- `section` (current route)
- `skillsAgents` (array)
- `connectors` (array with connection states)
- `projects` (array)
- `tasks` (array)
- `profileName`, `profileEmail`
- `userFlows` (array)
- `defaultAIProvider`, `modelTemperature`, `modelMaxTokens`, `modelSystemPrompt`
- `builderProjectName`, `builderChatMessages`, `builderUploadedAssets`

**Save Strategy:** Debounce setState → localStorage write on significant changes

---

## 🔐 Onboarding Flow

**Visible when** `onboarding === true`

**Overlay Modal:**
```
Position: fixed, inset 0
Background: rgba(0,0,0,0.92)
Backdrop filter: blur(6px)
Z-index: 100

Content: 440px max-width, centered
1. Logo
2. Welcome heading + description
3. Photo upload (drag-drop or click)
4. Full name input
5. Email input
6. Start button (disabled until name + email filled)
```

**On Complete:**
- Save name, email, photo to state
- Set `onboarding: false`
- Route to home section

---

## 🎬 Modals & Overlays

### Connector Setup Modal

**Triggered by:** "Connect" button on connector card

**Tabs:**
1. **Login / OAuth** – "Continue with [Provider]" button, mock success
2. **API Key** – Text input, optional secret, Save & Test button

**Behavior:**
- Mock OAuth: Show success message, save `connected: true` to localStorage
- API Key: Mask saved key, show disconnect option

### Change Password Modal

**Fields:**
- Current password
- New password
- Confirm password
- Save button

### Component Market Modal

**Structure:**
- Header: "Component Library" + close button
- Left sidebar: Category list
- Main grid: Infinite-scroll components (icon, name, Add button)

**Categories:**
- All
- Auth (Apple, Google, Two Factor)
- Hardware (Camera, Bluetooth, Microphone, Location)
- UI (Chat UI, Modal, Drawer, Calendar, Carousel)
- Tools (Bug Fixer, Analytics, Logger, Debugger)
- Backend (Database, API Client, WebSocket, Auth0)

---

## ✨ Interaction Details

### Sidebar Collapse

- Toggle chevron icon
- Smooth width transition (0.3s)
- Expanded: shows text labels
- Collapsed: icons only
- Main content resizes, logo hidden when collapsed

### Connector Icons in Composer

**Quick icons (home screen only):**
- Meta Ads (Facebook blue, #1877f2)
- Google Ads (gradient)
- YouTube Ads (white)
- More (dark, opens Connectors screen)

**Click behavior:**
- Each opens corresponding connector modal
- "More" routes to Connectors screen

### Publish Menu (Builder)

**Dropdown items:**
- Google Play Store
- Apple App Store
- Ship Flow

**On Click:**
- Show toast / confirmation state
- Mock success

### Builder Canvas Preview

**Phone Mockup:**
- Background image: `phone-mockup-portrait.png`
- Inner content area: 251px × 508px
- Display editable title, description
- Tweak panel controls live preview styling

---

## 📐 Responsive & Fixed Sizing

**Desktop Only** – No responsive breakpoints

**Fixed Widths:**
- Sidebar: 248px (expanded) / 64px (collapsed)
- Main viewport: `calc(100% - sidebarWidth)`
- Composer: max-width 860px
- Content sections: max-width 900px–1400px
- Right panel (builder): 300px fixed
- Left panel (builder): 320px fixed

---

## 🎨 CSS Reset & Global Styles

```css
* { margin: 0; padding: 0; box-sizing: border-box; }
html, body { 
  width: 100%; 
  height: 100%; 
  background: #000; 
  color: #e8e8e8; 
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; 
  font-size: 14px; 
  line-height: 1.5; 
  overflow: hidden; 
  -webkit-font-smoothing: antialiased; 
}
textarea, input, button { font-family: inherit; }
textarea::placeholder, input::placeholder { color: #6a6a6a; }
```

---

## 🚀 Implementation Notes for Claude Code

1. **Do not redesign.** Every pixel, color, spacing, and animation in this doc must match the output.
2. **Component-based architecture.** Build reusable components (Sidebar, Card, Button, Modal, etc.).
3. **Routing.** Use React Router or state-based section management.
4. **localStorage.** Persist app state with debouncing to avoid excessive writes.
5. **Markdown parsing.** Implement `.md` frontmatter parsing for Skills/Agents.
6. **Animations.** Use CSS `@keyframes` + Framer Motion for smooth, approved transitions.
7. **Icons.** Use Lucide or similar SVG icon library (not emoji in production UI).
8. **No backend yet.** All data lives in localStorage; implement mock flows.
9. **Type safety.** Full TypeScript with interfaces for Connector, Project, Task, etc.
10. **Test connectors.** Mock OAuth and API key flows — no real integrations.

---

## ✅ Checklist for Code Conversion

- [ ] React + TypeScript + Vite project initialized
- [ ] Sidebar component (collapsible, styled, icons + text)
- [ ] Home screen (composer, action chips, connector icons)
- [ ] Skills / Agents screen (grid, create, upload, preview, `.md` parsing)
- [ ] Library screen (upload, grid, preview, rename, delete)
- [ ] Connectors screen (grid by category, cards, mock modals)
- [ ] Projects & Tasks screens (list, open, continue)
- [ ] Settings screen (tabs: Profile, User Flows, AI Models, Workspace)
- [ ] Builder Workshop (3-panel: chat + canvas + tweaks)
- [ ] Routing (section-based or react-router)
- [ ] localStorage persistence (zouk-state key)
- [ ] Onboarding flow (photo, name, email → home)
- [ ] All colors, fonts, spacing from design system
- [ ] Animations: fadeIn, breathe glow, transitions
- [ ] Hover states, focus states, loading states
- [ ] Modal/overlay handlers
- [ ] Component market infinite scroll
- [ ] Markdown parsing for Skills/Agents
- [ ] Mock connector flows (OAuth, API key)
- [ ] All buttons wired to functions (no dead buttons)

---

**End of Reference Document**

This is your north star. Everything else is implementation detail. Do not deviate from these specs without explicit approval.
