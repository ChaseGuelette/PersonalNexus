# Spatial Layout

> Part of [[Algorithms]]

The hardest single algorithm in PersonalNexus: **place every node in 3D so that relational distance approximates spatial distance.**

> "Theres a component of this where we need to be building relational matricies between entries and then placing them in 3d space. I think of this conceptually as the same thing as the brain tree graph, but in 3D." — original notepad

## The Problem

Given:
- N nodes
- An association matrix (top-k similarities, see [[Association-Matrix]])
- A core anchor (Chase himself = origin)

Output:
- A `position: vec3` for every node such that highly-associated nodes are spatially close, and dissimilar nodes are far apart, while keeping the layout *legible* (not all clumped, not flying apart).

## Candidate Approaches

### 1. Force-Directed (Verlet / spring-electric)
- Each node is a charged particle (electrostatic repulsion)
- Each edge is a spring (attractive force, length proportional to 1 - assoc_score)
- Iterate until equilibrium

**Pros**: well-understood; nice organic look; runs in real time for moderate N
**Cons**: O(N²) naive; non-deterministic; can be unstable as graph grows
**Use for**: real-time interactive arrangements, fine-tuning local clusters

### 2. UMAP / t-SNE in 3D
- Dimensionality reduction from embedding space (384/1536d → 3d)
- UMAP preserves global + local structure better than t-SNE in our use case
- Run as a batch process; cache result

**Pros**: deterministic, reproducible; preserves semantic structure beautifully
**Cons**: expensive (seconds–minutes for thousands of nodes); doesn't naturally handle non-embedding signals (manual edges)

**Use for**: the canonical position when graph is mostly stable

### 3. Hybrid (RECOMMENDED)
1. Compute UMAP positions from embeddings → initial layout
2. Apply force-directed pass weighted by the full association matrix → adjusts for manual edges, themes, temporal proximity
3. Pin certain anchors (e.g., Chase's core orb is at origin; Mom's nexus has a fixed direction)

This combines UMAP's semantic faithfulness with force-directed's responsiveness to non-embedding signals.

## Anchoring

Some nodes have fixed positions:
- **Chase / self** = origin (0, 0, 0) — the core orb
- **Major theme axes** could be at fixed cardinal directions (childhood, family, work, philosophy)
- **Other nexuses** (Mom's, etc.) = fixed off-center positions

Everything else floats relative to those anchors.

## Stability Requirements

When a single new node is added, the existing layout should NOT shuffle. Users will be disoriented if "their" nexus rearranges itself.

**Strategy**: 
- Existing positions are anchors with high stiffness
- New node is positioned at the centroid of its top associations, then settled with force-directed
- Periodic full recomputation (rare, with user opt-in) to relieve accumulated drift

## Cross-Nexus Layout

When Chase's nexus and Mom's nexus both exist, they need to coexist spatially:
- Each nexus has its own internal layout
- The two are placed on a parent meta-graph
- Connections between them (e.g., "Chase ↔ Mom") are rendered as bridge-helixes
- Spatial separation between nexuses ≈ inverse of overall graph-graph similarity

(See [[Cross-Nexus-Linking]] for the topology details.)

## Open Questions
- 3D vs higher-dim-projected-to-3D? Should we lay out in 4D and render a 3D projection that the user can rotate?
- How often to recompute? Real-time, on every change, or batch?
- How do we visualize "uncertainty" in placement (a node we don't know where to put)?
