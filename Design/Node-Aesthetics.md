# Node Aesthetics

> Part of [[Design]]

How different kinds of nodes look. Each type has a distinct visual identity so the graph reads at a glance.

## Node Types

### Memory / Journal Entry
A small glowing polyhedron (octahedron or tetrahedron). Inner light is gold-warm if emotionally significant, blue-cool if reflective/intellectual. Faint particles drift around it. On hover, a single line of the entry appears next to it — never the whole text, only a fragment that beckons.

### Photo / Image
The photo itself, but framed in cosmic ornament — sacred geometry around the edges, slight bloom, a constellation pattern faintly overlaid. References: `fire_ahh_oldman.png` for the framing style.

### Video
Like a photo node but the frame contains a slow looping clip (a few key seconds). The frame breathes — pulses very slowly with the rhythm of the clip.

### Person
A constellation-portrait. The person's face (or a stylized version) sits inside an ornate ring of constellations. Person nodes are *anchors* — they tend to attract many connections. Hovering shows the person's name in serif glow text. (Reference: `fire_ahh_oldman.png`.)

### Idea / Symbol / Theme
A more abstract form — a glyph, a sigil, or a small piece of sacred geometry (icosahedron, tetrahedron, dodecahedron). Themes like "fragments," "mirrors," "family," "authors" are these. They are denser visually because they are concentrators of meaning.

### Place
A small landscape vignette inside a circular frame — like a tarot card. Switzerland might be a card showing mountains; "home" might show a doorway. Procedurally generated or user-supplied imagery.

### Event / Moment
Similar to a memory node but with a timestamp glow — a clock-face fragment behind it suggesting time. Connected events form a thread that visibly weaves through the nexus.

## Selection States

- **Default**: Faint glow, idle motion.
- **Hover**: Glow intensifies, subtle title appears, a single connection line lights up.
- **Selected**: Strong bloom, all connected lines illuminate, surrounding particles converge.
- **Focused** (deep dive): Full entry/page opens beside the node; the rest of the nexus dims to 30% opacity but remains visible (focus + context).

## The Core Orb

The center of the nexus. Likely an icosahedron (see `helix.png` reference). It represents Chase himself — the "I am here" anchor. From it, the major helixes emerge. It rotates slowly, always.

## Visual Density Rules
- Active region (where the user is): ~80% visual brightness
- Inactive periphery: ~20-30% brightness, still visible
- Selected node: 120% brightness with bloom
- Connections fade with distance — far links are ghosted, near links are bright
