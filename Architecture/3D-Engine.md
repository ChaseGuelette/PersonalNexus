# 3D Engine

> Part of [[Architecture]]

The rendering stack. This decision shapes everything visual.

## Leading Candidates

### 1. Three.js + React Three Fiber (RECOMMENDED for first prototype)
**Pros**:
- Largest ecosystem, most examples for the cosmic/glow look
- React Three Fiber gives a declarative scene graph — easier iteration
- Post-processing chain (bloom, glow, depth-of-field) is well-documented
- Many existing nebula/star-field/particle implementations to reference

**Cons**:
- WebGL means browser-bound (probably fine — the app is web-first)
- Performance ceiling lower than WebGPU once scale grows past ~100k nodes

**Verdict**: Best starting choice. Lets us prototype the visuals fast.

### 2. Babylon.js
**Pros**:
- More batteries included (PBR materials, GUI system, physics)
- Strong WebGPU support
- Better debug tools

**Cons**:
- Smaller cosmic-art ecosystem
- More opinionated; harder to iterate quickly on art direction

**Verdict**: Reasonable alternative. Could swap in if Three.js hits limits.

### 3. Custom WebGPU
**Pros**:
- Maximum performance ceiling
- Full control over shaders and pipeline

**Cons**:
- Massively higher effort. Wrong call for a first build.

**Verdict**: Don't. Future option only.

### 4. Native (Bevy / Godot / Unity)
**Pros**:
- Performance, polish, full art-pipeline tooling

**Cons**:
- Web distribution becomes harder. We lose the "open the URL, here's your nexus" experience.

**Verdict**: Not yet. Maybe a desktop port later.

## Initial Decision (provisional)
**Three.js + React Three Fiber + drei + postprocessing**, served from a Vite + React frontend. Revisit if we hit hard performance walls or if WebGPU support matures meaningfully.

## Key Libraries (R3F flavor)
- `three`
- `@react-three/fiber`
- `@react-three/drei` — common helpers (camera controls, sky, etc.)
- `@react-three/postprocessing` — bloom, vignette, depth-of-field
- `troika-three-text` — quality typography in 3D space (for entry text panels)
- `leva` — dev-only UI for tweaking visual parameters

## Camera and Controls
- `OrbitControls` from drei as a starting point
- Custom camera arcs for focus transitions (interpolate between orbit positions with spring physics)

## Performance Targets
- 60fps with 500 visible nodes + 1000 edges + bloom
- Graceful degradation past 1000 visible nodes (LOD kicks in)
- See [[Performance-Strategy]] for details.
