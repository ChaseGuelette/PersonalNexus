# MVP Scope

> Part of [[Build]]

The smallest version of PersonalNexus that proves the vision is real and worth pursuing.

## In Scope (MVP)

### Data Layer
- SQLite schema for nodes + edges
- Seed-loader that imports the initial entries from [[Content/Seed-Entries]]
- Basic CRUD for nodes via a local API
- Inspect / pair `ruvector.db` for embeddings

### Embeddings
- Local sentence-transformer embedding for text
- Basic cosine similarity → top-k associations per node

### Visualization
- 3D scene with:
  - Cosmic nebula background (procedural, no designable backdrops yet)
  - Core orb (icosahedron) at center, slowly rotating
  - At least 3 visible helixes from the orb
  - 5-10 nodes positioned by force-directed layout
  - Bloom + glow post-processing
- Camera: orbit + zoom (mouse drag + scroll)
- Click to focus a node → entry panel slides in beside it

### Ingestion
- A single "add entry" form (paste text + title) that:
  - Persists the entry as a node
  - Embeds it
  - Computes top-k associations
  - Renders the new node into the live scene with a smooth animation

### Aesthetic
- Looks unmistakably cosmic / not-a-todo-app
- 60fps with current node count
- Glow, bloom, slow ceremonial motion

## Out of Scope (defer)
- Image / video upload pipelines
- Face detection
- Cross-nexus linking (boundary nodes design only)
- LLM-based poetry analysis (basic NER tagging is enough for MVP)
- Designable backgrounds (the ticking watch — future)
- User-authored visual customization
- Tag taxonomy management UI
- Export / backup
- Mobile / responsive
- Authentication / multi-user

## The "Make Chase Feel Something" Test
The MVP isn't done when all the bullets above are checked. It's done when Chase opens it and feels the vision in his chest — when he wants to add the next entry just to see it appear. If the bullets are done but that feeling isn't there, the MVP needs more work on aesthetic, animation, or ceremony.

(See [[Vision/Success-Criteria]].)
