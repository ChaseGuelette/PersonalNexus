# Execution Plan

> Part of [[BRAIN-INDEX]]

The phased roadmap for PersonalNexus. This is the source of truth for *what to do next*. Update statuses as work progresses (`/wrap-up-braintree` will help).

The plan favors **small, ship-able milestones** over big-bang phases. Each phase ends with something on screen or in the database that proves the next phase is worth doing.

---

## Phase 1: Foundation (Data Layer)

**Goal**: A working SQLite + embedding pipeline that can hold and find seed entries. No UI yet.

| Step | Task | Status | Dependencies | Details |
|------|------|--------|--------------|---------|
| 1.1 | Inspect `ruvector.db` | not_started | None | Open the existing file, document its schema, decide whether to extend it |
| 1.2 | Choose vector store strategy | not_started | 1.1 | Single ruvector vs. paired with sqlite-vec — see [[Architecture/Storage]] |
| 1.3 | Set up project scaffold | not_started | None | Vite + React + TypeScript + Three.js + R3F dependencies |
| 1.4 | Create `nodes` and `edges` schemas | not_started | 1.2 | Per [[Architecture/Data-Model]] |
| 1.5 | Build text embedding pipeline | not_started | 1.3 | Local sentence-transformer; function `embed(text) -> float[384]` |
| 1.6 | Top-k similarity query function | not_started | 1.4, 1.5 | < 100ms target on 1000 nodes |
| 1.7 | Seed loader with initial nodes | not_started | 1.4, 1.5 | Loads Chase, Mom, Switzerland, theme nodes per [[Content/Seed-Entries]] |
| 1.8 | Tests for data layer | not_started | 1.7 | Embedding determinism, seed-load + similarity snapshot |

**Acceptance**: `npm run seed` loads the DB. CLI/REPL can query "top 5 similar to entry X" and the result is intuitively reasonable. All tests pass.

---

## Phase 2: First Visible Nexus

**Goal**: Open the app in a browser and see a cosmic 3D scene with real seed nodes.

| Step | Task | Status | Dependencies | Details |
|------|------|--------|--------------|---------|
| 2.1 | Cosmic background scene | not_started | 1.3 | Procedural nebula + parallax stars (see [[Design/Backgrounds]]) |
| 2.2 | Core orb (icosahedron) | not_started | 2.1 | Slow rotation, glow, at center (0,0,0) |
| 2.3 | Render seed nodes from DB | not_started | 1.7, 2.2 | Pull from API, render with type-specific geometry |
| 2.4 | Force-directed layout | not_started | 1.6, 2.3 | Initial positions; cache; anchor self at origin |
| 2.5 | Helix edges between connected nodes | not_started | 2.3, 2.4 | LOD-aware: full helix close, simple line far |
| 2.6 | Camera controls | not_started | 2.2 | Orbit + zoom; click background to reset |
| 2.7 | Bloom + selective glow postprocessing | not_started | 2.3 | Match the [[Design/Visual-Language]] palette |
| 2.8 | Click-to-focus animation | not_started | 2.6 | Camera arc to a node; surrounding context dims |
| 2.9 | Entry panel UI | not_started | 2.8 | Illuminated-manuscript style; shows body, tags, connections |
| 2.10 | Connection follow (camera arc to clicked link) | not_started | 2.8, 2.9 | Maintain trail; reflow panel |
| 2.11 | FPS counter (dev) | not_started | 2.1 | Bottom-right overlay |

**Acceptance**: Open browser → cosmic scene loads → seed nodes visible → click a node → focus + panel → click a connection → smooth transition. 60fps target hit on a moderate laptop.

---

## Phase 3: Live Ingestion

**Goal**: Add new entries through the app and watch them appear in the nexus.

| Step | Task | Status | Dependencies | Details |
|------|------|--------|--------------|---------|
| 3.1 | "New entry" form | not_started | 2.9 | Markdown editor or paste-and-submit textbox |
| 3.2 | Submit endpoint: persist + embed + top-k | not_started | 1.5, 1.6 | Wraps the Phase-1 pipeline |
| 3.3 | Auto-tag proposal (NER + keywords) | not_started | 3.2 | Surface analysis only, no LLM yet |
| 3.4 | Connection proposal review UI | not_started | 3.3 | Show top-k as suggested connections; user confirms |
| 3.5 | Animated insertion of new node into scene | not_started | 2.4, 3.4 | New node drifts into position; helixes weave outward |

**Acceptance**: Chase types an entry → submits → sees suggestions → confirms a few → sees the new node materialize in 3D with its connections. The graph *grew* and you felt it.

---

## Phase 4: Aesthetic Pass

**Goal**: Make it feel like art, not a tech demo.

| Step | Task | Status | Dependencies | Details |
|------|------|--------|--------------|---------|
| 4.1 | Tune nebula shader for the [[Design/Visual-Language]] palette | not_started | 2.1 | Indigo/violet/electric blue, slow drift |
| 4.2 | Helix detail and animation | not_started | 2.5 | Real DNA-twist geometry, particles flowing along edges |
| 4.3 | Person-node constellation framing | not_started | 2.3 | Match `fire_ahh_oldman.png` visual treatment |
| 4.4 | Ceremonial motion easing | not_started | 2.6 | All transitions on slow cubic-bezier curves |
| 4.5 | Typography pass for entry panels | not_started | 2.9 | Serif (Cinzel/Cormorant), warm gold accents |
| 4.6 | Selection bursts and pulses | not_started | 2.8 | Constellation burst on selection |
| 4.7 | Self-test: does it make Chase feel something? | not_started | 4.1-4.6 | The Real Test — see [[Vision/Success-Criteria]] |

**Acceptance**: When Chase opens it, he wants to add the next entry just to see it appear. The vision is real on screen.

---

## Phase 5: Multi-Modal (post-MVP)

**Goal**: Image and video ingestion, then poetry-style analysis.

| Step | Task | Status | Dependencies | Details |
|------|------|--------|--------------|---------|
| 5.1 | Image upload + thumbnail generation | not_started | Phase 3 | Per [[Content/Image-Pipeline]] |
| 5.2 | CLIP embeddings for photos | not_started | 5.1 | Image similarity search |
| 5.3 | Photo-node rendering with cosmic framing | not_started | 5.1 | Match [[Design/Node-Aesthetics]] |
| 5.4 | Face detection and person-tagging | not_started | 5.2 | Local model (no cloud) |
| 5.5 | Video upload + Whisper transcript | not_started | 5.1 | Per [[Content/Video-Pipeline]] |
| 5.6 | LLM-powered theme extraction | not_started | Phase 3 | Local Llama or API; per [[Algorithms/Poetry-Analysis]] |
| 5.7 | Connection proposal upgrades from theme analysis | not_started | 5.6 | Replace surface NER with deeper analysis |

**Acceptance**: Drop a photo → it joins the nexus with smart auto-connections. LLM analyzer reads new entries and proposes thematic links the cosine similarity alone would miss.

---

## Phase 6: Backgrounds + Cross-Nexus (long-horizon)

**Goal**: Designable backdrops (the ticking watch) and a second connected nexus.

| Step | Task | Status | Dependencies | Details |
|------|------|--------|--------------|---------|
| 6.1 | Designable background system | not_started | Phase 4 | User can swap in custom backdrop designs |
| 6.2 | Real-time ticking watch backdrop | not_started | 6.1 | The first user-authored backdrop — see [[Design/Backgrounds]] |
| 6.3 | Region-based star color shifts | not_started | 6.1 | Color zones tied to nexus regions |
| 6.4 | Boundary node implementation | not_started | Phase 3 | Per [[Algorithms/Cross-Nexus-Linking]] |
| 6.5 | Second nexus (Mom-nexus stub) | not_started | 6.4 | At least one boundary connection live |
| 6.6 | Inter-nexus camera transition | not_started | 6.5 | "Walk through the gateway" UX |

**Acceptance**: The ticking watch is in the cosmos behind the nexus. Chase can travel into Mom's nexus and back.

---

## Working Notes

- Phases are not strict sequence — Phase 1 and Phase 2 can overlap in practice (set up scaffold while doing data work).
- Within a phase, ship steps as soon as they're done. No long-lived branches.
- After every phase, do a [[Vision/Success-Criteria]] check: are we still on the path?
- The Real Test (Vision/Success-Criteria) is the only test that matters. Pass everything else and fail that, the project hasn't shipped.
