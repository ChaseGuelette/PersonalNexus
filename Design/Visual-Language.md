# Visual Language

> Part of [[Design]]

The vocabulary of forms, colors, and behaviors that PersonalNexus is built from.

## Palette

**Primary**: Deep indigo and ultramarine for void/background. Electric blue and violet for connections and helixes. Gold and warm white for sacred-geometry highlights and selected/active states.

**Avoid**: Pure black (use deep indigo instead). Pure white (warm off-white only). Greens, oranges, reds (except sparingly, intentionally — never as defaults). Flat saturation.

**Light**: Bloom and glow are core. Every meaningful object emits light. Connections shimmer faintly even when idle.

## Materials and Surfaces

- Helixes: glassy, glowing, semi-translucent. Inner light visible.
- Core orb: facets like a cut gem. Icosahedron is the leading candidate (see [[Vision/Inspirations]] — `helix.png` reference).
- Person nodes: portrait-in-frame style, with sacred geometry surrounds (see `fire_ahh_oldman.png` for the look).
- Memory/entry nodes: small luminous shapes — possibly tetrahedra or octahedra, glowing from within.
- Photo nodes: the photo itself, framed in cosmic ornament.

## Motion

- Slow, ceremonial. Nothing snaps.
- Helixes rotate slowly, always.
- The core orb spins steadily.
- Selected nodes pulse faintly.
- Transitions are eased — `cubic-bezier(0.4, 0.0, 0.2, 1)` or slower.

## Particles

- Ambient drift particles in the background (nebula dust)
- Connection lines emit small particles flowing along their path (data-as-living-current)
- Node selection bursts a small constellation outward, then settles

## Typography

- Avoid flat-modern sans. Lean toward serifs with a ceremonial weight (Cinzel, EB Garamond, Cormorant) for headers and entry titles.
- Body text: a clean readable serif or a low-contrast humanist sans (Inter is too flat — try Spectral or Source Serif).
- All text glows faintly. Light gold or warm white on dark.

## Anti-Patterns
- Hard-edged Material Design buttons
- Flat saturated colors
- Drop shadows (use bloom/glow instead)
- "Empty state" illustrations of cartoon people
- Anything that looks like a SaaS dashboard
