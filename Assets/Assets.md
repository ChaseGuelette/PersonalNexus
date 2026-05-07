# Assets

> Part of [[BRAIN-INDEX]]

This is the brain's pointer to visual reference material. The actual files live in the project's `images/` folder at the repo root (NOT inside this brain folder), keeping the existing structure intact.

Drop new visual references into `images/`. Add an entry below describing what the image captures and how it relates to the project.

## How It Works
When working on any visual / design / build task, agents check `images/` for related materials. You can analyze images directly. Drop in screenshots, mockups, photos, anything.

## Supported Formats
- Images (PNG, JPG, WebP, AVIF, SVG): screenshots, references, mockups, photographs
- Videos (MP4, MOV): demos, recordings
- PDFs: documents, papers
- 3D model files (later)

## Naming Convention
Use descriptive names where possible: `helix-icosahedron-reference.png` is better than `IMG_1234.png`. Existing files (some with random IDs) are kept; new additions should follow the convention.

## Current Visual References (in `../images/`)

### Cosmic Backdrop / Watch
- `5557689e-c041-4cb1-a0e5-8334109f41d8.png` — Watch in deep nebula. Reference for the background-watch concept (see [[Design/Backgrounds]]).
- `watchface.jpg` / `watchface.avif` — Holzkern wood-and-blue watch face. Texture and ornament inspiration.

### Connection Tissue
- `dna.jpg` — Pure DNA helix on dark blue. Reference for connection-line glow and twist.
- `helix.png` — **Most important reference.** Icosahedron core with DNA helixes radiating, in nebula. Closest match to the actual nexus look.

### Sacred Geometry
- `icosahedron.jpg` — Clean line drawing.
- `icosahedron-line-drawing-sacred-geometry-260nw-288304808.webp` — Vector form, sacred-geometry style.

### Person Nodes
- `fire_ahh_oldman.png` — Portrait framed in constellations. Visual treatment for `person` nodes (see [[Design/Node-Aesthetics]]).
- `mna.jpg` — Same portrait, raw, no cosmic treatment. "Before" reference.

### Navigation
- `skyrimskillselection.jpg` — Skyrim cosmic skill tree. The focus-plus-context navigation paradigm (see [[Design/Navigation-Model]]).

### Starting Point Comparison
- `braintreeos2d.png` — A 2D brain-tree graph. The flat baseline that PersonalNexus is *evolving past* into 3D + art.

## How References Map to Brain Files

| Reference | Used by |
|---|---|
| `helix.png` | [[Design/Visual-Language]], [[Design/Node-Aesthetics]] |
| `5557689e-c041-...png`, `watchface.*` | [[Design/Backgrounds]] |
| `skyrimskillselection.jpg` | [[Design/Navigation-Model]] |
| `fire_ahh_oldman.png` | [[Design/Node-Aesthetics]] (person nodes) |
| `dna.jpg` | [[Design/Visual-Language]] (connection lines) |
| `icosahedron*` | [[Design/Node-Aesthetics]] (core orb) |
