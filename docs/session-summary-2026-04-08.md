# Nested Living — Session Summary

**Date:** 2026-04-08
**Project:** Nested Living Interactive Illustration
**Live URL:** https://nestedliving.com
**Fallback URL:** https://nested-living.pages.dev
**GitHub:** https://github.com/vbha006/nested-living

---

## What Was Built

A single-file interactive SVG illustration depicting a Buckminster Fuller-inspired geodesic dome community, rendered as a bird's-eye/drone-shot view of an Auckland-style landscape. The entire application lives in one self-contained HTML file (~1,100 lines) with no external dependencies beyond Google Fonts.

## Starting Point

The session began with an existing `nested-living.html` that had:
- Three geodesic domes **nested concentrically** (all centered at x=700) — regional, community, and family domes stacked inside each other
- Hover-triggered tooltip popups for information
- Static, non-moving cars and bikes on a transport layer
- No people/pedestrian layer
- A right-side slide-out info panel (narrow, vertical)
- Five toggleable flow layers: solar, water, nutrients, food, transport

## Changes Made

### 1. Landscape Overhaul — Vast Auckland Drone Shot

The entire scene was reimagined as a wide, expansive landscape rather than a compact nested diagram:

- **Harbour strip** across the horizon (Waitemata-inspired) with subtle ripple lines and four sailboats
- **Rangitoto volcanic cone** silhouette on the harbour horizon (right of center)
- **Auckland CBD cluster** with a recognisable Sky Tower hint (thin spire + observation deck circle)
- **Rolling hills** spanning the full 1400px canvas width, replacing the previous two isolated mountain groups
- **Wider soil cross-section** (x:140 to x:1300) spanning beneath all domes
- **Trees redistributed** across the broader landscape between dome sites
- Stars (dark mode), clouds, and sun/moon repositioned for the wider composition

### 2. Domes Separated Across the Landscape

The three dome scales were pulled apart into distinct locations:

| Dome | Position (cx) | Radius | Role |
|------|--------------|--------|------|
| Regional | 320 (left) | 148px | Orchards, aquaculture pond, large-scale food |
| Community | 740 (center) | 104px | Food commons plaza, shared infrastructure |
| Family A/B/C | 1040/1145/1250 (right cluster) | 42px each | Private gardens, individual living |

Each dome retains its own:
- Geodesic grid (clip-pathed)
- Solar panels at the crown
- Rainwater channels
- Interior garden beds (family) / orchard rows (regional) / plaza plants (community)
- Compost station beside regional dome, cistern beside community dome

### 3. All Flow Paths Rewritten

Every animated flow path was redrawn to connect the separated domes:

- **Solar (5 paths):** Sun fans out to each dome crown individually (no cascading through nested layers)
- **Water (5 paths):** Clouds deliver rain to nearest domes; underground pipes (pW4, pW5) link regional→community→family
- **Nutrients (7 paths):** Underground soil cycle arcs between domes + root channels down from each dome base
- **Food (4 paths):** Family surplus flows to community commons, then onward to regional
- **Transport (2 paths):** Winding road (pT1 forward, pT2 reverse) curves past every dome
- **People (2 paths):** Gentler walking path (pPe forward, pPe2 reverse) slightly above the road

### 4. Information Panel — Bottom Landscape Layout

Replaced the narrow right-side slide-out panel with a full-width bottom panel:

- **CSS grid:** 3-column layout (260px header | 1fr body | 300px metrics)
- **Slides up from bottom** with spring easing (`cubic-bezier(0.34,1.5,0.64,1)`)
- **238px tall**, with backdrop blur
- Header column: icon + title + subtitle (with left border separator)
- Body column: scrollable descriptive HTML content
- Metrics column: key-value pairs (with right border separator)
- Close button (x) top-right

### 5. Hover Tooltips Removed — Click-Only Interaction

- Deleted the `#tip` tooltip DOM element and its CSS
- Removed all `mouseenter`, `mousemove`, `mouseleave` event listeners from `bindHot()`
- Retained click-to-open-panel behavior on `.hot` elements
- SVG background click closes the panel
- Added clickable label badges for transport and people layers (pill-shaped SVG rects with text)

### 6. Animated Vehicles (Cars & Bikes Actually Moving)

Replaced static positioned bikes/cars with `<animateMotion>` driven movement:

- **`drive()` helper function:** Attaches `animateMotion` + `mpath` to any SVG group
- **5 forward bikes** on pT1 (speeds 14-20s per loop, staggered starts)
- **3 reverse bikes** on pT2
- **2 forward shared EVs** on pT1 (slower, 22-26s)
- **1 reverse EV** on pT2
- Bikes now include a **rider silhouette** (head circle + body line)
- Road rendered as a wide curved stroke path (not a flat rect), matching the winding route

### 7. New People Layer

Entirely new layer with its own toggle button, legend entry, and INFO data:

**Walking figures:**
- Stick-figure `person()` function: head, torso, legs, arms
- Subtle **bobbing animation** (`animateTransform` translate, 0.55s cycle)
- 6 walkers on forward path (pPe), 4 on reverse (pPe2), varied speeds (28-39s)

**Stationary workers:**
- 4 figures at regional dome (orchard workers)
- 4 at community dome (plaza activity)
- 2 at each family dome (residents)

**UI integration:**
- Layer button: `🚶 PEOPLE` with magenta active state (`#8A2068`)
- Legend entry with magenta color swatch
- INFO panel entry with description of daily life, walking distances, community scale
- Clickable label badge: "PEOPLE — DAILY LIFE IN THE COMMONS"

### 8. INFO Data Addition

New `people` entry added to the INFO object:
- Icon: 🚶
- Title: PEOPLE & DAILY LIFE
- Subtitle: Social Fabric — Residents Moving Between Home, Commons & Landscape
- Body: Two paragraphs on walking culture, community interaction, human-scale distances
- Metrics: Primary mode (Walking), Max walk time (~10 min), Commons visits (Multiple/day), Community size (Human-scale)

## Deployment

### Cloudflare Pages
- **Project:** `nested-living`
- **Account:** `997fd2c103d6db02c482e08cb3db3792` (Varunlecturer)
- Deployed via `wrangler pages deploy .`
- Both `nested-living.html` and `index.html` (copy) uploaded

### Custom Domain — nestedliving.com
- **Zone created** via Cloudflare API (zone ID: `cad5262c7ac65011d3a6b0b47c107e4a`)
- **Nameservers assigned:** `david.ns.cloudflare.com`, `romina.ns.cloudflare.com`
- **DNS records added:**
  - `nestedliving.com` → CNAME → `nested-living.pages.dev` (proxied)
  - `www.nestedliving.com` → CNAME → `nested-living.pages.dev` (proxied)
- **Pages custom domains registered:** both `nestedliving.com` and `www.nestedliving.com`
- **SSL:** Auto-provisioned by Cloudflare (Google CA)
- **GoDaddy nameservers** updated manually by user to Cloudflare NS

### GitHub Repository
- **Repo:** https://github.com/vbha006/nested-living (public)
- **Branch:** `main`
- **Commits:**
  1. `beb331f` — Initial commit: Nested Living interactive illustration
  2. `96d6216` — Add .gitignore for wrangler cache
- `.gitignore` excludes `.wrangler/`

## Technical Architecture

### File Structure
```
nested_living/
├── nested-living.html    # Source file (canonical)
├── index.html            # Copy for Cloudflare Pages root serving
├── docs/
│   └── session-summary-2026-04-08.md
└── .gitignore            # Excludes .wrangler/
```

### SVG Coordinate System
- ViewBox: `0 0 1400 780`
- Ground line (GY): `562`
- Sky fills from y:0 to GY+14
- Underground/soil: GY to GY+70
- Transport road: ~GY+22 to GY+40
- People path: ~GY+8 to GY+14

### Key Constants (in `build()`)
```
R_cx=320, R_cy=GY, R_r=148    // Regional dome
C_cx=740, C_cy=GY, C_r=104    // Community dome
FA_cx=1040, FB_cx=1145, FC_cx=1250, F_cy=GY, F_r=42  // Family domes
sunX=1320, sunY=80             // Sun/moon position
```

### Layer System
Six toggleable layers, each an SVG `<g>` with id `layer-{name}`:
- `solar` — energy flow paths + particles
- `water` — rain + underground water pipes + particles
- `nutrients` — soil cycle + root channels + particles
- `food` — surplus flow from family → community → regional
- `transport` — road, moving bikes/cars
- `people` — walking figures + stationary workers

### Theme System
- CSS custom properties on `:root[data-theme=light|dark]`
- `cl(light, dark)` helper in JS returns the appropriate color
- Full rebuild on theme toggle (SVG is reconstructed)
- `applyLayers()` re-applies layer visibility after rebuild

### Animation Techniques
- **Dashed flow lines:** CSS `@keyframes` with `stroke-dashoffset`
- **Particles along paths:** `<animateMotion>` with `<mpath>` referencing `<path>` elements in `<defs>`
- **Vehicle/person movement:** Same `animateMotion`/`mpath` pattern via `drive()` helper
- **Person bobbing:** `<animateTransform>` translate on each figure group

## Redeployment

To redeploy after changes:
```bash
cd c:/Users/dabun/nested_living
cp nested-living.html index.html
CLOUDFLARE_ACCOUNT_ID=997fd2c103d6db02c482e08cb3db3792 npx wrangler pages deploy . --project-name nested-living
```
