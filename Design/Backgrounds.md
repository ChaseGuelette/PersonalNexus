# Backgrounds

> Part of [[Design]]

The cosmos in which the nexus lives. Backgrounds are not blank space — they are part of the artwork.

## Default Background: Living Nebula

A procedurally generated deep-space nebula. Slow color drift over time. Stars occupy multiple parallax layers. As the user orients/rotates around the nexus, the visible portion of the cosmos changes — like turning your head in a planetarium.

**Implementation hints (for [[builder]])**:
- Skybox / cubemap with multi-layer noise (Perlin/Simplex) for nebula clouds
- Star particles in 3-4 depth layers with parallax
- Color drift via a slow palette interpolation (e.g., indigo → violet → cyan → indigo, ~10min cycle)

## Star Color Behavior

> "I want the background to be a mixture of stars, but as you scroll the color of those stars changes."

As the camera moves through 3D space, stars shift hue. This isn't random — it's a function of position. Different "regions" of the nexus space have different cosmic moods.

**Idea**: Map nexus regions to color zones. Memories of childhood = warmer (gold-white-pink). Recent/active thoughts = cooler (electric blue). Symbolic/poetic regions = violet. Family region = warm with gold.

## Designable Background Designs

> "I want the background to be designable. Like I want to be able to incorporate different designs into the stars. ... I want to have a giant watch in the background that is ticking in real time. As you orient yourself around the nexus, that watch changes."

Backgrounds support **layered design objects** — large-scale ornaments embedded in the cosmos behind the nexus.

**The ticking watch**:
- Real mechanical watch face rendered in deep nebula behind the nexus
- Hands tick in real time (the user's actual time)
- As camera moves, different parts of the watch face become visible
- Multiple watches possible — different "rooms" of the nexus could have different watches

**Other potential backdrop designs** (for the future):
- Constellation patterns spelling out personally meaningful symbols
- A slow orbiting planet visible from one angle of the nexus
- Sheet music drifting across the void
- Architectural lattices reminiscent of memory palaces

## User-Authored Backgrounds

Long-term: Chase can drop image/SVG/3D-model files into a background slot, and the system embeds them in cosmic ornament — auto-generates the framing, the glow, the parallax layers — so any visual artifact can become part of the cosmos.
