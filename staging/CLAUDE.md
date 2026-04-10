# Geometric Pattern Generator — CLAUDE.md

## Project Overview
A collection of browser-based generative art tools built with p5.js, published via GitHub Pages. Tools include mandala, kaleidoscope, wave pattern, fireworks, and planetary system generators.

## Branch & Deploy Workflow

| Branch | Purpose | URL |
|--------|---------|-----|
| `staging` | Active development | `jeffellenbogen.github.io/Geometric-Pattern-Generator/staging/` |
| `main` | Production source | `jeffellenbogen.github.io/Geometric-Pattern-Generator/` |
| `gh-pages` | Pages deploy target — do not push manually | managed by Actions |

**Rules:**
- Always work on and push to `staging`. GitHub Actions auto-deploys to the staging URL.
- Never open a PR from `staging` → `main` unless the user explicitly requests it.
- Never push directly to `gh-pages`.

## Versioning

- Version is defined as `const VERSION` in `planetary-generator.html` and displayed in the controls panel top-right above "Time & View".
- **Patch** (third number): bump on every bug fix or minor change — e.g. `v2.0.0` → `v2.0.1`.
- **Minor / Major**: only bump when the user explicitly requests it.

## Planetary Generator — Key Technical Notes

File: `planetary-generator.html`

**Position calculation:**
- Planet angles use **mean longitude** at J2000.0 (not mean anomaly). Mean longitude = mean anomaly + longitude of perihelion, giving correct absolute angular position in the solar system.
- Epoch: J2000.0 = January 1.5, 2000. Code subtracts 1.5 days from the Jan 1.0 day count to align.
- `daysFromEpoch` drives `meanLongitudeAtEpoch + (daysFromEpoch / period) * TWO_PI`.

**Planet data fields:**
- `period` — orbital period in days (precise values, e.g. Earth = 365.25)
- `eccentricity` — real orbital eccentricity from JPL
- `meanLongitudeAtEpoch` — radians at J2000.0 from JPL orbital elements

**Known limitations:**
- Uses mean longitude as position angle (not true anomaly), so residual angular error exists — largest for Mercury (~10°), small for others (<2°).
- Orbital inclinations ignored; all planets drawn in the same plane.
- Orbital distances are compressed for visualization (outer planets especially).

**Accuracy testing:**
- Dec 21, 2020: Jupiter & Saturn should appear nearly overlapping (great conjunction).
- Oct 13, 2020: Earth & Mars should be on the same side of the Sun (opposition).
- Reference tool: Stellarium Web or JPL Horizons (`ssd.jpl.nasa.gov/horizons`).

## Stack
- p5.js v1.4.0 (via CDN)
- p5.js-svg v1.5.1 for SVG export
- Plain HTML/CSS/JS — no build step
