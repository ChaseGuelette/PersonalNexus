# Phase 2: Visualization

> Part of [[Build]]

Get the first visible nexus on screen. This is where the project becomes real.

## Goals
- A 3D scene rendering with the cosmic aesthetic
- Real seed nodes from Phase 1's database appearing as visualizations
- Basic camera controls
- An entry panel that opens when a node is clicked
- 60fps target hit on a typical laptop

## Deliverables

### 1. Project Scaffold
- Vite + React + TypeScript
- Three.js + React Three Fiber + drei + postprocessing
- Tailwind for the 2D UI surfaces
- Path aliases, lint, basic CI

### 2. Scene Skeleton
- `<Canvas>` with the cosmic background:
  - Procedural nebula (shader or layered noise)
  - Star particles in 3-4 parallax layers
- Slow rotating core orb (icosahedron, see [[Design/Node-Aesthetics]])
- Ambient drift particles

### 3. Node Rendering
- Pull seed nodes from the DB via a thin API
- Render each node according to its `type`:
  - Entry → octahedron with glow
  - Person → constellation-portrait frame (placeholder texture for MVP)
  - Idea → small geometric glyph
  - Place → tarot-card vignette (placeholder)

### 4. Edges
- Render confirmed edges as glowing helix-style lines
- Helix detail at high LOD (close camera), simple lines at low LOD (far camera)

### 5. Spatial Layout
- Use `d3-force-3d` or a custom force-directed pass to place nodes
- Anchor: Chase = (0,0,0)
- Run once on load; cache positions

### 6. Camera
- `OrbitControls` from drei
- Click on background → reset to default orbit
- Click on node → animate camera to focus on it

### 7. Entry Panel
- When a node is selected, a 2D panel slides in (e.g., right side or floating beside the node)
- Shows title, body, tags, themes, and outgoing connections
- Connections are clickable → camera arcs to the connected node, panel reflows

### 8. Postprocessing
- Selective bloom on bright objects
- Vignette (subtle)
- Optional: depth of field for focused-node mode

### 9. FPS Counter (dev only)
- Bottom-right overlay, dev-mode only
- Track FPS, draw calls, triangle count

## Done Criteria
- Run `npm run dev` → scene loads in browser
- Cosmic background, orb, and helixes visible
- Seed nodes from the DB appear in 3D
- Click a node → focus + entry panel
- 60fps on a moderate laptop with the seed graph

## Estimated Effort
1-2 weeks. Most time goes into the procedural nebula + bloom tuning to match the aesthetic. The data plumbing is small in this phase.
