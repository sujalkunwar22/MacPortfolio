# Technical Specification — Sujal Kunwar macOS Portfolio

## Dependencies

| Package | Version | Purpose |
|---------|---------|---------|
| (none) | — | Zero external dependencies per requirements. All code is vanilla HTML5, CSS3, and ECMAScript in a single `index.html` file. |

---

## Browser APIs Used

- **localStorage** — Persist theme preference, window positions, first-visit boot flag
- **requestAnimationFrame** — 60fps drag, resize, and minimize animations
- **CSS Custom Properties** — Theme switching, dynamic colors, spacing tokens
- **CSS backdrop-filter** — Glassmorphism on windows, dock, menu bar
- **CSS transitions / @keyframes** — All UI animations
- **DOM API** — createElement, appendChild, querySelector, classList, dataset
- **Event API** — addEventListener for mouse, touch, keyboard events
- **Canvas 2D API** — Terminal Matrix rain effect
- **setInterval** — Clock updates, boot sequence timing
- **matchMedia** — Responsive breakpoint detection

---

## Component Inventory

### Layout Components (always mounted)

| Component | Source | Notes |
|-----------|--------|-------|
| BootScreen | Hand-written | Cinematic boot sequence; removes self from DOM after completion |
| DesktopWallpaper | Hand-written | CSS gradient background with 20s position animation; two variants for light/dark |
| MenuBar | Hand-written | Glassmorphic top bar; clock interval, theme toggle, app name sync |
| Dock | Hand-written | Glassmorphic bottom strip; icon magnification via CSS `:hover` sibling selectors; running dots |
| SpotlightOverlay | Hand-written | Modal search overlay; indexes all app content; keyboard navigation |

### Window Components (lazy-mounted on first open)

| Component | Source | Notes |
|-----------|--------|-------|
| WindowFrame | Hand-written | Reusable macOS window shell: glass surface, traffic lights, title bar drag handle, resize grip, minimize/maximize/close. Accepts content HTML as innerHTML or child nodes. |
| FinderWindow | Hand-written | Extends WindowFrame. Sidebar nav (Favorites/Recents/Locations) + main content canvas with profile, tech stack, interests grid. |
| TerminalWindow | Hand-written | Extends WindowFrame. Interactive command parser with canvas-based Matrix rain easter egg. |
| SafariWindow | Hand-written | Extends WindowFrame. Browser chrome (address bar, nav buttons, tabs) + project cards grid. |
| MailWindow | Hand-written | Extends WindowFrame. Email composition form with validation and success notification. |

### Reusable Components (within windows)

| Component | Source | Notes |
|-----------|--------|-------|
| TrafficLights | Hand-written | Three colored circles (red/yellow/green) with hover icons. |
| CalendarDropdown | Hand-written | Date display + monthly grid. Shows on clock click. |
| NotificationToast | Hand-written | Slide-in success/error messages (e.g., "Message Sent!"). |

### Hooks/Utilities (vanilla JS functions)

| Function | Purpose |
|----------|---------|
| `makeDraggable(el, handle, bounds)` | Mouse/touch drag with bounds enforcement and GPU-accelerated transforms |
| `makeResizable(el, handle)` | Bottom-right resize grip with min constraints |
| `WindowManager` | Global z-index stack, active window tracking, open/close/minimize/restore registry |
| `ThemeManager` | Toggle `data-theme` on `<html>`, persist to localStorage, swap wallpaper |
| `Clock` | 60s interval updating clock text, calendar grid generation |
| `SpotlightEngine` | Real-time content indexing, fuzzy search, result rendering |
| `BootController` | Orchestrated multi-phase boot sequence with setTimeout chain |
| `debounce(fn, ms)` | Generic debounce for search input |
| `getHighestZIndex()` | Scan all windows, return max z-index + 1 |

---

## Animation Implementation

| Animation | Library | Implementation Approach | Complexity |
|-----------|---------|------------------------|------------|
| Boot logo fade-in | CSS @keyframes | `opacity: 0→1` over 800ms | Low |
| Boot logo pulse | CSS @keyframes | `scale(1)→scale(1.03)→scale(1)` over 600ms | Low |
| Boot logo fade-out | CSS @keyframes | `opacity: 1→0` over 500ms | Low |
| Boot screen dissolve | CSS @keyframes | Overlay `opacity: 1→0` over 800ms, then DOM removal | Low |
| Menu bar slide-in | CSS transition | `translateY(-28px)→translateY(0)`, 500ms ease-decel | Low |
| Dock slide-in | CSS transition | `translateY(80px)→translateY(0)`, 500ms ease-spring, 200ms delay | Low |
| **Window drag (60fps)** | **Vanilla JS + rAF** | Mousedown stores offset; mousemove updates `transform: translate3d(x, y, 0)` via rAF; bounds clamping on all edges. Touch events supported. | **High** |
| **Window minimize** | **Vanilla JS + CSS** | Compute vector from window center to dock icon position; animate `scale3d(0.05,0.05,1) translate3d(dockX, dockY, 0)` over 400ms ease-accel; hide on complete. | **High** |
| **Window restore** | **Vanilla JS + CSS** | Reverse minimize: animate from dock position back to stored coords over 350ms ease-spring. | **High** |
| **Window maximize** | **Vanilla JS + CSS** | Animate to `top:28px left:0 width:100vw height:calc(100vh-28px-64px-12px)` over 300ms ease-smooth. Toggle on second click. | **Medium** |
| Window open bounce | CSS @keyframes | Dock icon `translateY(0)→-12px→0` over 400ms ease-spring | Low |
| Window focus (z-index) | Vanilla JS | Click sets `zIndex = getHighestZIndex() + 1`; applies active shadow. | Low |
| Traffic light hover icons | CSS transition | `opacity: 0→1` on `:hover`, 120ms ease-default | Low |
| Dock magnification | CSS `:hover` + `~` selectors | Hovered icon `scale(1.3)`, adjacent `scale(1.15)`, second-adjacent `scale(1.05)`. 200ms ease-spring transition. | Medium |
| Running dot glow | CSS | `box-shadow: 0 0 4px rgba(255,255,255,0.5)` static | Low |
| Theme transition | CSS transition | All themed properties transition 300ms ease-default on `data-theme` attribute change | Low |
| Clock dropdown | CSS transition | `opacity 0→1`, `translateY(-4px)→0`, 150ms ease-decel | Low |
| **Spotlight open/close** | **CSS + JS** | Backdrop `opacity 0→0.4`, search bar `scale(0.97)→1` + `opacity 0→1`, 150ms. Close reverses. | **Medium** |
| **Mail send slide-out** | **CSS + JS** | Form fields `translateX(0)→-100%` with stagger, 300ms; notification card slides in from right `translateX(100%)→0`, 400ms ease-spring. | **Medium** |
| **Terminal Matrix rain** | **Canvas 2D API** | `requestAnimationFrame` loop drawing green character columns falling at varied speeds. Random char selection from katakana + latin. Clear and restart on command. | **High** |
| Coffee emoji explosion | Vanilla JS | Generate 100+ ☕ emojis with random positions, animate `opacity 1→0` and `scale 1→0` with staggered delays over 2s | Medium |
| Wallpaper gradient shift | CSS @keyframes | `background-position` shifts across 200% over 20s, infinite linear | Low |
| Safari card hover | CSS transition | Card `translateY(0)→-4px`, shadow deepens, 200ms ease-default | Low |
| Mail validation shake | CSS @keyframes | Form `translateX(0)→-8px→8px→-8px→8px→0` over 400ms on validation error | Low |
| Notification toast | CSS transition | `translateY(-100%)→0` + `opacity 0→1`, 400ms ease-spring | Low |

---

## State & Logic Plan

### WindowManager (Core System)

This is the central nervous system. All window lifecycle, focus, z-index, and positioning flow through it.

**Registry Pattern:**
- Maintain `windows` Map keyed by app name (`finder`, `safari`, `terminal`, `mail`)
- Each entry stores: `{ element, isOpen, isMinimized, isMaximized, preMinimizeRect, preMaximizeRect, zIndex }`
- Global `activeWindow` reference

**Focus Logic:**
- `focusWindow(appName)` → Set target z-index to current max + 10, add `window-active` class, remove from others, update Menu Bar app name
- `blurAll()` → Remove `window-active` from all, reset Menu Bar to "Finder"

**Open/Close/Minimize/Maximize State Machine:**
- `open(appName)` → If closed: create window DOM (lazy), set initial position (cascaded from last opened), animate in. If minimized: restore instead.
- `close(appName)` → Animate out or hide directly, mark closed, remove running dot if no other windows
- `minimize(appName)` → Store current rect, animate to dock icon, hide, mark minimized, show running dot
- `maximize(appName)` → If not maximized: store rect, animate to fullscreen. If maximized: restore stored rect.

**Critical Implementation Detail:**
- Minimize animation requires computing the exact screen coordinates of the target dock icon element to set the `translate3d` destination. This needs a `getDockIconPosition(appName)` utility that reads the icon's `getBoundingClientRect()`.

### Spotlight Search Index

**Indexing Strategy:**
On init, build a static search index array containing objects like:
```js
{ type: 'app', app: 'finder', label: 'Profile', keywords: 'about me sujal kunwar profile nepal' }
{ type: 'app', app: 'finder', label: 'Tech Stack', keywords: 'javascript css html python curriculum' }
{ type: 'app', app: 'safari', label: 'Recipe Sharing Engine', keywords: 'recipe web project' }
{ type: 'command', app: 'terminal', label: 'help', keywords: 'help commands' }
{ type: 'command', app: 'terminal', label: 'skills', keywords: 'skills technical expertise' }
// ... etc
```

**Search Logic:**
- Debounced 50ms input listener
- Simple `toLowerCase().includes(query)` across all `keywords` fields
- Filter and render results in real-time
- Click handler calls `WindowManager.open(result.app)` and closes spotlight

### Terminal Command Parser

**State:**
- `history[]`: Array of past `{ command, output }` objects
- `historyIndex`: For up/down arrow navigation through history
- `isMatrixRunning`: Boolean flag to stop animation loop

**Parser Logic:**
- Input captured on `keydown` (Enter to submit, prevent form behavior)
- Switch/case on trimmed lowercase command string
- Each command appends a new output block to the terminal buffer DOM
- `clear` removes all children except the active prompt line
- `hack` sets `isMatrixRunning = true`, launches canvas animation loop. Any subsequent keypress or `clear` sets `isMatrixRunning = false` and clears canvas.
- `coffee` generates emoji elements with randomized positions and fade-out animations

**Matrix Rain Implementation:**
- Single `<canvas>` element sized to terminal content area
- `requestAnimationFrame` loop
- Columns array: each column has `{ x, y, speed, chars[] }`
- Draw semi-transparent black rectangle each frame for trail effect
- Draw random green characters (`String.fromCharCode` range for katakana + latin) at column positions
- Increment `y` by `speed`, reset to 0 when past canvas height
- Stop condition: `isMatrixRunning === false`

### Theme Manager

**State:**
- `theme`: `'light'` | `'dark'`, read from `localStorage.getItem('theme')` or defaults to `'light'`

**Logic:**
- `toggle()` → Flip theme, set `document.documentElement.dataset.theme`, write to localStorage, swap desktop wallpaper class
- Init on DOMContentLoaded: read persisted theme and apply immediately (before first paint if possible)

### Clock System

**State:**
- `isDropdownOpen`: boolean

**Logic:**
- `updateClock()` → Format `new Date()` to `12:30 PM` style, update DOM text
- `setInterval(updateClock, 60000)` + immediate call on init
- Calendar dropdown: Generate current month grid (7×n), highlight today with blue circle
- Close on outside click or Escape

### Mail Form Validation

**Logic:**
- On submit (send button click):
  1. Validate "To" field: Regex check for basic email format (`/^\S+@\S+\.\S+$/`)
  2. Validate body: Non-empty after trimming
  3. If invalid: Apply shake animation to form, do not submit
  4. If valid: Animate form fields sliding left, show notification toast "Message Sent Successfully!"
- No actual email sending — this is a UI simulation

---

## Other Key Decisions

### Single-File Architecture

The entire application (HTML structure, CSS styles, JS logic) lives in one `index.html` file. This means:
- CSS is in a `<style>` block in `<head>`
- JS is in a `<script>` block at end of `<body>`
- All SVG icons are inline `<svg>` elements (no external fetches except wallpaper and dock icon images)
- Window content HTML is built via `document.createElement` and `innerHTML` in JS, not pre-defined in HTML

### Lazy Window Construction

Windows are not in the initial HTML. They are created dynamically on first open:
- Reduces initial DOM size and paint time
- Heavy components (Terminal canvas, Safari project cards) only load when needed
- Window positions cascade: each new window opens offset +30px/+30px from the last

### Responsive Strategy

Use `window.matchMedia('(max-width: 768px)')` checked at init and on resize:
- Below 768px: Override all window styles to `position:fixed; top:28px; left:0; width:100vw; height:calc(100vh-28px-64px)`; disable drag/resize listeners
- Touch events are always registered alongside mouse events — no conditional needed

