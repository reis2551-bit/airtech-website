# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static website for **AirTech**, an air conditioning installation/maintenance/repair company in Zona Centro, Portugal. It is a proposal/delivery for a client — content is in European Portuguese.

## Stack

- **Zero dependencies** — pure HTML/CSS/JS, no build step, no npm, no frameworks
- Two files: `index.html` (live website) and `PDF.html` (A4 printable proposal document)
- Served by opening the file directly in a browser or deploying to any static host (Netlify, cPanel, etc.)

## Architecture: Single-Page Application via Hash Routing

`index.html` is a self-contained SPA. All 9 pages live as `<div id="page-{name}">` elements inside one HTML file. Navigation is handled by `goTo(id)` in the inline `<script>` at the bottom:

```
pages = ['home','sobre','servicos','instalacao','manutencao','reparacao','testemunhos','contactos','orcamento']
```

- `goTo(id)` removes the `.active` class from all pages and adds it to the target — CSS `.page { display:none }` / `.page.active { display:block }` handles visibility.
- The URL hash (`#home`, `#orcamento`, etc.) is updated via `history.replaceState`, enabling direct linking.
- On load, the hash is read and the matching page is activated.

## Design System (CSS Custom Properties)

All colours, spacing, and shadows are defined as CSS variables in `:root` at the top of `index.html`:

```css
--blue: #2563EB   /* primary CTA */
--navy: #0F172A   /* headings, dark backgrounds */
--green: #059669  /* success, check icons */
--border: #E5E7EB
--bg2: #F9FAFB    /* alternating section backgrounds */
```

`PDF.html` uses a separate, slightly different palette (sky blue accent `#0EA5E9`, lighter navy `#0C1A2E`) — keep the two files visually consistent if both are edited.

## Key Patterns

- **Typography scale**: `.label` → `.h1/.h2/.h3` → `.lead` → `.body-text` — use these classes, do not add inline font-size.
- **Buttons**: `.btn` + modifier (`.btn-primary`, `.btn-secondary`, `.btn-green`, `.btn-lg`, `.btn-sm`, `.btn-block`).
- **Cards**: `.card` with optional `.card-green` modifier. Hover lift is built into the class.
- **Scroll animations**: An `IntersectionObserver` at the bottom of the script watches `.card, .test-card, .value-item, .step-item, .about-val` and fades them in. New elements of these types animate automatically once they match those selectors.
- **Forms**: Fields use `.form-group > label + input|select|textarea`. Side-by-side fields use `.form-row` (collapses to 1-col on mobile).

## Placeholder Content to Replace for Production

When delivering to the client, these placeholders must be swapped for real values:

| Placeholder | Location |
|---|---|
| `+351 239 000 000` | navbar top bar, all service pages, footer, WhatsApp links |
| `https://wa.me/351239000000` | floating WhatsApp button, contact page, quote page |
| `info@airtech.pt` | contact page, footer |
| Google Maps embed `src` | `#page-contactos` |
| Google Business link for reviews | `#page-testemunhos` |
| Unsplash image URLs | hero, service detail pages |

## Development Workflow

No build tooling. Open either file directly in a browser:

```bash
# Windows — open in default browser
start index.html
start PDF.html

# Or serve locally to avoid CORS issues with external resources
python -m http.server 8080
```

To print `PDF.html` to PDF: open in browser → Ctrl+P → Save as PDF (A4, no margins).
