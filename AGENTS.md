# AGENTS.md

Instructions and codebase reference for AI coding sessions in **AnimeFlashcards** (`FlashCards de Estudo`).

## Project Overview

AnimeFlashcards is a zero-dependency, static client-side **Progressive Web App (PWA)** built with vanilla HTML5, CSS3, and ES6+ JavaScript. It renders a responsive grid of 3D-flipping study flashcards for memorizing anime (and selected video game) characters, with real-time progress tracking, shuffling/resetting, session-only custom card creation with local image upload, and PWA install/App Shell caching support.

- **UI Language:** Brazilian Portuguese (`lang="pt-BR"`). Keep all user-facing strings in Portuguese.
- **Code Identifiers:** English (`camelCase` in JS/HTML IDs, `kebab-case` in CSS).
- **Existing Documentation:** Preserve [README.md](./README.md) (written in Portuguese, includes all-rights-reserved license notice).

## Repository Structure

```text
.
├── index.html            # Single-page entry point, layout, controls, tooltip, and "Novo Card" modal
├── style.css             # All styling, CSS custom properties, 3D card flip animations, responsive grid
├── script.js             # Hardcoded sampleCards dataset (53 items), state, DOM rendering, modal, PWA install & SW registration
├── service-worker.js     # App Shell precaching and cache-first fetch handler
├── manifest.webmanifest  # PWA manifest (standalone mode, theme colors, SVG icon)
├── icons/
│   └── icon.svg          # 512x512 vector app icon (also used as favicon and apple-touch-icon)
├── README.md             # Project documentation (pt-BR)
└── .gitignore            # Standard Visual Studio gitignore template
```

## Technologies & Dependencies

- **Runtime:** Browser-native HTML5, CSS3 (Grid, Custom Properties, 3D Transforms), Vanilla JavaScript (ES6+).
- **Web APIs Used:** DOM API, `FileReader` (`readAsDataURL`), `localStorage`, Service Worker API, Cache Storage API, `window.matchMedia`, `beforeinstallprompt`, `appinstalled`.
- **Build/Package Tools:** None. There is no `package.json`, bundler, transpiler, CSS preprocessor, or test framework.
- **External Network Resources:**
  - Google Fonts (`Playfair Display` and `DM Sans`) loaded in `index.html`.
  - Character portrait URLs in `sampleCards` (`script.js`) point to external hosts (Fandom/Wikia, Wikimedia, Reddit, Pinterest, etc.) and load with `loading="lazy"`. They are **not** precached by `service-worker.js`.

## Build, Test & Run Commands

- **Build:** Not required. Edit static files directly.
- **Automated Tests / Linting:** None configured. Verify changes manually in a browser and via DevTools (Console + Application tab for Service Worker / Manifest / Cache Storage).
- **Run Locally:**
  - **Direct open:** Open `index.html` in a browser (works for core flashcard UI, but Service Worker registration and PWA install require HTTP/HTTPS).
  - **Local HTTP server (recommended for PWA testing):**
    ```bash
    # Python
    python -m http.server 8000

    # Node.js
    npx serve .
    ```
    Alternatively, use the VS Code Live Server extension.

## Architecture & Key Decisions

### 1. Classic Global Script + Inline Event Handlers
- `script.js` is loaded via `<script src="script.js" defer></script>` (not `type="module"`).
- `index.html` and the card HTML generated in `render()` bind interactions via inline attributes (`onclick="flipCard(this)"`, `onclick="resetAll()"`, `onclick="shuffleCards()"`, `onclick="openModal()"`, `onclick="closeModal()"`, `onclick="addCard()"`, `onclick="handleOverlayClick(event)"`, `onchange="previewImage(event)"`).
- PWA banner and lifecycle events (`installButton`, `installDismiss`, `beforeinstallprompt`, `appinstalled`, `load`) use `addEventListener`.
- **Guideline:** Do not convert `script.js` to an ES module or hide handler functions inside a closure unless you also migrate all inline event attributes in `index.html` and `render()` to `addEventListener`.

### 2. State Management & Card Schema
- State in `script.js` is purely in-memory:
  - `cards`: shallow copy of `sampleCards` (`let cards = [...sampleCards]`).
  - `flippedSet`: `Set` storing **0-based array indices** of currently flipped cards (`data-index="${i}"`).
- Custom cards added via the modal are **not persisted** across page reloads; only install-banner dismissal is stored in `localStorage` under `'flashcards-install-dismissed'`.
- Because `flippedSet` tracks indices rather than unique IDs:
  - `addCard()` prepends with `cards.unshift(...)` and shifts every index in `flippedSet` by `+1`.
  - `resetAll()` and `shuffleCards()` clear `flippedSet` and call `render()`.
- Initialization calls `render({ shuffle: true })`, which renders once and immediately invokes `shuffleCards()` (triggering a second `render()`).
- **Card object shape (`sampleCards` and `addCard()`):**
  ```js
  {
    name: "Character Full Name", // Required; shown on card back and reveal tooltip
    desc: "Short name or note",  // Optional; rendered if truthy
    tag: "Anime or Game Title",  // Optional; rendered as pill badge if truthy
    imageUrl: "https://..."      // Optional URL, Data URL, or null (renders .placeholder-img)
  }
  ```

### 3. Service Worker & Cache Busting
- `service-worker.js` caches `APP_SHELL`:
  `['./', './index.html', './style.css', './script.js', './manifest.webmanifest', './icons/icon.svg']`
- Fetch strategy for `GET` requests is **cache-first** (`caches.match(req) || fetch(req)`), with `skipWaiting()` on `install` and old cache cleanup + `clients.claim()` on `activate`.
- **Critical Guideline:** Whenever you modify any cached file (`index.html`, `style.css`, `script.js`, `manifest.webmanifest`, `icons/icon.svg`), bump `CACHE_NAME` in `service-worker.js` (currently `'flashcards-shell-v1'`) so returning PWA users receive the update. If you add new local static assets, add their relative paths to `APP_SHELL`.
- Keep all asset and manifest paths **relative** (`./...`, `icons/icon.svg`) so deployment works under subpaths (e.g., GitHub Pages).

### 4. Styling System (`style.css`)
- Design tokens live in `:root` (`--bg`, `--surface`, `--card-front`, `--card-back`, `--accent`, `--accent-dim`, `--text-primary`, `--text-muted`, `--border`, `--gold-glow`, `--radius`, `--bg-card`). Reuse these variables rather than hardcoding colors.
- Typography: `'Playfair Display', serif` for display headings, stat counters, and revealed card names; `'DM Sans', sans-serif` for UI controls and metadata.
- 3D card flip relies on `.card-wrapper` (`perspective: 1000px`), `.card` (`aspect-ratio: 3/4`, `transform-style: preserve-3d`), `.card.flipped` (`transform: rotateY(180deg)`), and `.card-face` (`backface-visibility: hidden`).
- Layering (`z-index`): grain overlay `body::before` (`0`) < main content (`1`) < `.tooltip` (`100`) < `.modal-overlay` (`200`) < `.install-banner` (`300`).
- Responsive layout: fluid CSS Grid (`repeat(auto-fill, minmax(220px, 1fr))`) with a single breakpoint at `@media (max-width: 520px)` switching to `repeat(2, 1fr)`.

## Coding Conventions & Guidelines

- **Formatting:** 2-space indentation across HTML, CSS, JS, and JSON. Preserve existing section comment headers (`// ─── Section ───` in `script.js`, `/* Section */` in `style.css`).
- **Naming:**
  - JS variables/functions and HTML `id` attributes: `camelCase` (`flipCard`, `updateStats`, `cardGrid`, `modalOverlay`).
  - Service worker constants: `UPPER_SNAKE_CASE` (`CACHE_NAME`, `APP_SHELL`).
  - CSS classes and custom properties: `kebab-case` (`card-wrapper`, `progress-fill`, `--accent-dim`).
- **DOM Rendering Safety:** `render()` and `previewImage()` currently assemble HTML with `innerHTML` template strings without escaping (`card.name`, `card.desc`, `card.tag`, `card.imageUrl`, `file.name`). When editing rendering or user-input handling, avoid introducing XSS vectors (prefer `textContent` / DOM element construction or proper escaping).
- **Known Existing Quirks (avoid unintended side effects unless specifically asked to fix):**
  - `sampleCards` contains a duplicate `"Senua"` entry, minor tag typos/casing differences (`"Chawsaw man"`, `"Ousama ranking"`), and a trailing space in `"Yami "`.
  - `shuffleCards()` uses `cards.sort(() => Math.random() - 0.5)` (non-uniform shuffle).
  - `pendingImageUrl` in `script.js` is declared and reset in `openModal()` but unused (`pendingImageDataUrl` is used instead).
- **Scope Discipline:** Keep the project zero-dependency unless explicitly asked to introduce tooling or frameworks. Keep changes minimal, preserve existing features and relative paths, and remember to verify both normal browser load and Service Worker behavior.
