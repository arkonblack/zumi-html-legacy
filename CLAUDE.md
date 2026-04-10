## Project Overview

APP-ENVAX is a Point of Sale (POS) change calculator web app for Colombian peso (COP) transactions. It helps cashiers calculate change (vuelto) and track daily sales. The project consists of three standalone HTML files — no build system, no backend, no dependencies to install.

## Running the App

Open any HTML file directly in a browser — no server required:
- `App_v1.html` — Dark sidebar layout (ChatGPT-inspired)
- `App_v2.html` — Light iOS-style layout
- `App_v3.html` — Ultra-minimal mobile-first layout (latest)

## Architecture

Each file is a fully self-contained SPA using:
- **Vanilla JS + HTML5 + CSS3** — no frameworks
- **Tailwind CSS** via CDN
- **Lucide Icons** via CDN
- **localStorage** for data persistence (no backend)

### Core Functional Modules (same across versions)

| Module | Purpose |
|---|---|
| `processRealTimeInput()` / `processInput()` | Number formatting with dot separators; supports `+` operator to sum multiple amounts |
| `calcularVueltoFinal()` / `calcular()` | Computes change, validates payment sufficiency |
| `generarDesglose()` | Breaks down change into COP denominations |
| `registrarVenta()` | Persists transaction to localStorage |
| `renderHistorial()` | Renders transaction history grouped by date |

### Currency/Locale Conventions

- Locale display: `'de-DE'` formatting (dots as thousands separator, e.g. `1.000`)
- Denomination set (hardcoded): `[100000, 50000, 20000, 10000, 5000, 2000, 1000, 500, 200, 100]`
- All UI text in Spanish; Colombian Spanish locale (`es-CO`)

### localStorage Keys

- `App_v1.html` → `ventas_db`
- `App_v2.html` → `historial_sencillo`

## Development Notes

When adding features or creating a new version:
- Keep everything in a single HTML file (inline CSS + JS)
- The `+` operator in inputs lets users sum multiple received amounts before calculating
- Escape key clears the form
- Negative change result should render visually distinct (red) to signal insufficient payment
