# Association Matrix

> Part of [[Algorithms]]

The pairwise relationship score between every two nodes. This is the master signal that drives spatial layout and connection rendering.

## What It Is

For every pair of nodes (A, B), compute a score `assoc(A, B) ∈ [0, 1]` indicating how strongly they are related. Higher = closer in 3D space and possibly visualized with a stronger helix.

## Sources of Association Score

The score is a weighted combination of multiple signals:

### 1. Embedding Similarity (cosine)
- Compute cosine similarity of node embeddings
- Captures *semantic* relatedness ("authors" entry and "fragments" entry have similar vectors)
- Weight: ~0.4

### 2. Explicit Edges (manual + confirmed)
- If the user has drawn or confirmed an edge between A and B, that's a strong signal
- Bonus: edge type carries weight (`mentions` weaker than `family_of`)
- Weight: ~0.3

### 3. Shared Tags / Themes
- Jaccard similarity of tag sets
- Themes weighted higher than casual tags
- Weight: ~0.15

### 4. Co-Mention
- If A is mentioned in B's body (or vice versa), strong signal
- Weight: ~0.1

### 5. Temporal Proximity
- Entries written near each other in time often relate (same trip, same mood phase)
- Decay function on time delta
- Weight: ~0.05

These weights are starting heuristics. They should be tunable via a dev panel — and ultimately learned from Chase's manual confirmations of suggested links.

## Storage

Don't compute on demand. Precompute the matrix and store it. For N nodes, the full matrix is N² entries — at N=10,000 that's 100M cells, way too much.

**Strategy**: Store only the **top-k associations per node** (k = 50 or 100). Sparse representation. When we need similarity to a node, look up its top-k. For "rare" relationships beyond top-k, fall back to on-demand cosine computation (cheap).

## Recomputation Triggers
- Node added → compute its top-k against all other nodes
- Node body changed → recompute its embedding, recompute its top-k, update top-k of nodes that *had* this node in their list
- Edge confirmed/added → bump association score for that pair
- Manual recompute (cron / button) → full rebuild

## Open Questions
- Should we use a learned weight scheme (tune weights from Chase's confirmation behavior)?
- How to handle node types with no body text (photos)? Image embeddings only? Or fall back to caption + tags?
- Do we want negative edges ("these things feel opposite")? Probably not in v1.
