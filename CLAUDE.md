## Project Overview

**Zumi** is a Point of Sale (POS) change calculator web app for Colombian peso (COP) transactions. It helps cashiers calculate change (vuelto) and track daily sales. The active file is `index.html` — a single self-contained file with no build system, no backend, and no dependencies to install.

## Running the App

Open `index.html` directly in a browser — no server required.

Legacy versions (no longer actively maintained):
- `App_v1.html` — Dark sidebar layout (ChatGPT-inspired)
- `App_v2.html` — Light iOS-style layout
- `App_v3.html` — Ultra-minimal mobile-first layout

## Architecture

Single self-contained SPA (`index.html`) using:
- **Vanilla JS + HTML5 + CSS3** — no frameworks
- **Tailwind CSS** via CDN
- **Inter font** via Google Fonts
- **localStorage** for data persistence (no backend)

### Layout

Two-panel layout: collapsible sidebar (`#sidebar`) + main content area (`main`).

- **Sidebar** — logo, nav (Ventas / Notas), scrollable history, footer with balance + dark mode toggle + clear history button. Collapses to icon-only mode.
- **Main** — two views: `#view-calc` (calculator) and `#view-notas` (notes list), toggled via `irACalc()` / `irANotas()`.

### Core Functional Modules

| Module | Purpose |
|---|---|
| `processInput()` | Real-time number formatting with dot separators; supports `+ - * /` expressions |
| `evaluarExpresion()` | Evaluates arithmetic expressions respecting operator precedence |
| `calcular()` | Computes change, validates payment sufficiency, updates result display |
| `registrar()` / `guardarYLimpiar()` | Saves transaction to localStorage (Avanzado / Precision mode) |
| `renderSidebarVentas()` | Renders transaction history grouped by date in sidebar |
| `renderNotas()` | Renders notes list with filters, pending/resolved sections |
| `toggleModo()` / `setModo()` | Switches between Precision and Avanzado calculator modes |
| `toggleDarkMode()` / `updateDarkModeUI()` | Toggles dark mode with CSS View Transition animation |
| `toggleSidebar()` | Collapses/expands sidebar |

### Calculator Modes

- **Precision** — standard mode: enter total + received amount, calculates change. Button saves + clears.
- **Avanzado** — quick-cash mode: bill buttons (`$2k`–`$100k`) add to received field; shows denomination breakdown of change. Register button saves manually.

### Dark Mode

- Toggled via button in sidebar footer (moon/sun icon).
- Uses CSS `html.dark` class with CSS variable overrides (`--text`, `--bg`, `--surface`, etc.).
- Animated with the CSS View Transitions API — expanding circle from top-left corner.
- Preference persisted in `localStorage` key `dark_mode`.
- Anti-flash script in `<head>` applies class before render.

### Notes (Notas)

- Lightning bolt FAB (`⚡`) opens quick-note modal.
- Notes have optional color-coded tags; tags get auto-assigned colors from a fixed palette.
- Filter chips at top of notes view filter by tag.
- Notes toggle between `pendiente` / `resuelto` states.
- Keyboard shortcut: `n` opens modal; `Enter` saves; `Esc` closes.

### Currency/Locale Conventions

- Locale display: `'de-DE'` formatting (dots as thousands separator, e.g. `1.000`)
- Denomination set (hardcoded): `[100000, 50000, 20000, 10000, 5000, 2000, 1000, 500, 200, 100]`
- All UI text in Spanish; Colombian Spanish locale (`es-CO`)

### localStorage Keys

| Key | Contents |
|---|---|
| `historial_avanzado` | Array of sale transactions |
| `notas_db` | Array of notes |
| `etq_colores` | Map of tag name → hex color |
| `modo_app` | `'precision'` or `'avanzado'` |
| `sidebar_open` | `'true'` or `'false'` |
| `dark_mode` | `'dark'` or `'light'` |

## Development Notes

When adding features:
- Keep everything in a single HTML file (inline CSS + JS)
- CSS variables (`--text`, `--text2`, `--text3`, `--bg`, `--surface`, `--surface2`, `--border`, `--divider`) must be used for all structural colors so dark mode works
- JS-generated inline styles must use `var(--text2)` etc., not hardcoded hex values
- SVG icons in sidebar use `stroke="currentColor"` — color is controlled via CSS `.s-btn .s-icon svg` rules
- Negative change renders in red (`#FF3B30`); this color is intentionally kept hardcoded (same in both themes)
- Escape key clears the form (or closes the note modal if open)

## AI Custom Skills

### Skill: Subir a GitHub
Cuando el usuario pida "enviar a github", "subir cambios" o similares, debes ejecutar la siguiente secuencia sin dudar:
1. Verifica el estado con `git status`.
2. Agrega los cambios con `git add .`
3. Haz un commit con `git commit -m "Descripción concisa de los cambios realizados"`.
4. Sube los cambios con `git push origin main`.
*Nota: Si el push es rechazado porque hay cambios remotos, haz `git pull --rebase origin main` antes de volver a intentar el push.*
