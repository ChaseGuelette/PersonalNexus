# Navigation Model

> Part of [[Design]]

How users move through the nexus. The model is **focus + context** — always see the whole, always be able to dive deep.

## Reference: Skyrim Skill Tree

The cleanest example of the navigation we want. (See `skyrimskillselection.jpg`.)

- Constellations of glowing nodes against a deep cosmic backdrop
- You can move between constellation-clusters smoothly
- When you focus on one cluster, the others remain visible — at the edges of vision, in soft focus
- Selecting a node feels weighted, ceremonial — not a click, a *consideration*

## Camera Model

- **Free orbit** around the nexus center (the core orb).
- **Zoom** in/out smoothly (no hard zoom levels).
- **Focus** on a node: camera arcs toward it, recenters on it, but pulls back enough that surrounding context is still visible at the periphery.
- **Always-visible spine**: the core orb and primary helixes are *always* on screen at some intensity, even when the user is deep in a sub-region. The user is never "lost in a submenu."

## Scrolling and Movement
- Mouse drag: orbit
- Scroll wheel: zoom
- Click node: focus on it
- Click background: pull back to default orbit
- Keyboard arrows / WASD: pan camera (for power users)

## Reading an Entry (The "Open" State)

When a node is fully selected, the entry's content panel slides in alongside it. The panel:
- Sits in 3D space, floating beside the node
- Is rendered as an illuminated-manuscript-style page (serif text, gold accents, faint glow)
- Does NOT cover the nexus — the rest of the cosmos remains visible behind/around it (~30% opacity)
- Shows the entry text, attached photos, tags, and a list of *outgoing connections* the user can click to follow

## Following a Thread

Clicking a connection in an open entry doesn't close the panel and reload — the camera arcs to the connected node, the panel reflows to the new entry, and the trail of where you came from glows behind you for a moment. Reading should feel like wandering through cosmic corridors, not like clicking through web pages.

## Onboarding (First-Time Experience)
The user opens the app and sees the orb and a few helixes against the void. A single soft instruction: *"This is you. Add your first thought."* No tutorial, no walkthrough. The space teaches itself through interaction.
