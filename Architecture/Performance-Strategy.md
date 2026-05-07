# Performance Strategy

> Part of [[Architecture]]

> "How to keep the whole thing running without lag" — a stated concern in the original notepad.

PersonalNexus must run at 60fps with hundreds of visible nodes and bloom-heavy post-processing. This requires planning, not patching later.

## Frame Budget (16.6ms total)

| Stage | Budget |
|---|---|
| Scene update (positions, animations) | 2ms |
| Render pass | 8ms |
| Postprocessing (bloom, vignette) | 4ms |
| UI / React reconciliation | 1.5ms |
| Slack | 1.1ms |

If any single stage blows budget, we degrade — never drop frames.

## Strategies

### 1. Levels of Detail (LOD)
- Far nodes: rendered as point sprites (cheap)
- Mid-range nodes: low-poly geometry, no individual material
- Close nodes (in focus region): full geometry + materials + animation

Threshold distances are tunable per node type. Person nodes get richer detail at greater distance because they're anchors.

### 2. Frustum + Occlusion Culling
- Three.js does frustum culling by default — make sure we don't break it with custom shaders
- Camera-far is bounded; nodes beyond render distance are skipped

### 3. Instanced Rendering
- Stars, ambient particles, distant edges: rendered as instanced meshes (one draw call for thousands of objects)
- Only "near" / "active" nodes get individual draws

### 4. Edge Simplification
- Helixes are expensive. Render only the helixes connected to currently-visible-or-near regions in full detail.
- Distant edges become simple glowing lines, not helix-twists.

### 5. Lazy Loading
- Don't load every entry's full text into memory at once
- Embed metadata only; load body text when a node is hovered/selected
- Photos: load thumbnails first; full resolution only on selection

### 6. Off-Main-Thread Work
- Force-directed layout / embedding similarity → web worker
- Image processing (thumbnail generation, face detection) → server-side or worker
- Never block the render thread for graph computation

### 7. Bloom Selective
- Bloom is expensive when applied to the whole scene
- Use selective bloom (objects above a brightness threshold) — cheaper and looks better

## Performance Budgets to Track

- **Visible nodes**: ≤ 500 with full detail
- **Total scene nodes**: ≤ 5000 in DOM/Three at any time (rest are virtualized)
- **Edges visible**: ≤ 2000
- **Bundle size**: ≤ 1.5MB initial JS (lazy-load the rest)

## Profiling Cadence
- Add an FPS counter from day one (don't ship without it during dev)
- Run a Chrome DevTools profile every time we add a major scene element
- Spike-test with synthetic data (10k nodes) early — find walls before content fills them
