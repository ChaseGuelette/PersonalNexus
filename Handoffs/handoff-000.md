# Handoff 000: Brain Initialization

> Part of [[Handoffs]]

**Date**: May 07, 2026 at 04:13 PM
**Session**: Initial brain setup
**Owner**: Chase

## Summary

PersonalNexus brain initialized. Vision crystallized from the existing `notepad.md` and the visual references in `images/`. Brain structure created with 9 top-level folders (Vision, Design, Architecture, Algorithms, Content, Build, Assets, Templates, Handoffs), 4 agent personas (builder, architect, artist, curator), an execution plan, templates, and ~30 content files filled with substantive material — not placeholder.

## Context Captured

### The Vision
PersonalNexus is a 3D interactive knowledge graph — journal + notepad + museum + interactive art. A spinning orb in space with DNA-helix connections, deep cosmic aesthetic, Skyrim-skill-tree navigation, designable backdrops including a real-time ticking watch in deep nebula. Multiple nexuses (Chase's, Mom's) connect via boundary nodes.

It is **not** a productivity tool. It is for self-witnessing.

### Source Material Read
- `notepad.md` (full): Chase's articulation of the vision, including the Switzerland diary example
- `images/` folder (10 references): cosmic watches, DNA helixes, icosahedrons, Skyrim skill tree, constellation-framed portraits, 2D brain-tree comparison

### Visual Anchor
The single most important reference is `images/helix.png` — an icosahedron core with DNA helixes radiating in deep nebula. That's the closest match to what the actual nexus should look like.

## Decisions Made

1. **Folder structure**: Vision / Design / Architecture / Algorithms / Content / Build / Assets / Templates / Handoffs. Architecture and Algorithms are split — the math deserves its own home given Chase explicitly flagged it as "a huge algorithm" in the notepad.
2. **Provisional 3D engine**: Three.js + React Three Fiber + drei + postprocessing. Documented in [[Architecture/3D-Engine]]; revisit if needed.
3. **Provisional embedding model**: `all-MiniLM-L6-v2` local. Documented in [[Algorithms/Embeddings]]; evaluate against Chase's real entries before committing.
4. **Cross-nexus architecture**: Architecture B (linked nexus graphs with boundary nodes), not federated single graph. Privacy and aesthetic mapping cleaner. Documented in [[Algorithms/Cross-Nexus-Linking]].
5. **MVP scope**: Text-only ingestion + 3D scene with seed nodes. Image/video pipelines and LLM analysis deferred. Documented in [[Build/MVP-Scope]].

## What's in Place
- BRAIN-INDEX hub
- Updated CLAUDE.md (preserved OpenCode/GLM workflow, filled in vision/owner/structure sections)
- 4 agent personas (builder, architect, artist, curator) joining the existing explorer
- 9 fully-populated folders
- Execution Plan with phased table-format roadmap
- 3 templates (decision log, feature spec, daily note)
- Brain registered in `~/.braintree-os/brains.json` and switched from "building" to "live"

## Recommended Next Steps

1. **Inspect `ruvector.db`** ([[Build/Open-Questions]] flagged this as the first concrete code task). Open the file, document its schema in [[Architecture/Storage]], decide on extension vs. pairing.
2. **Provide the real Switzerland diary entry** (currently only described in summary). It's the canonical seed entry for the nexus — without it, MVP launch has no anchor content.
3. **Run `/sprint-braintree`** to plan the first concrete week of work.
4. **OR run `/plan-braintree Phase-1-Foundation`** to do detailed planning on the data layer build.
5. Consider dropping any additional visual references (especially actual desired-look mockups) into `images/`.

## Notes for Future Sessions

- The aesthetic is non-negotiable. Any feature that makes this feel like a productivity tool is wrong. (See [[Vision/Non-Goals]].)
- Always use [[builder]] for implementation, [[architect]] for design questions, [[artist]] for visual decisions, [[curator]] for content/tagging work.
- The OpenCode + GLM handoff pattern is preserved in [[CLAUDE.md]] — use it for code edits over ~20 lines.
- This brain is intimate-data-bound by intent — keep ML local where possible, see [[Vision/Non-Goals]].
