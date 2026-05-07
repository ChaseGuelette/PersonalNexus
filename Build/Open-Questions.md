# Open Questions

> Part of [[Build]]

Things we don't know yet that need answers before they block work. Living document — add to it freely.

## Architectural

- **What is `ruvector.db`?** Need to inspect the existing file and decide whether it's the canonical vector store or whether we replace it with `sqlite-vec`.
- **Single SQLite or split DBs?** One unified `nexus.db` vs. separate stores for graph and vectors. Probably depends on the answer to the question above.
- **Local LLM choice for poetry analysis.** Llama 3.x via Ollama, Phi-3.5, Mistral? Need to test on Chase's actual entries to see which one captures themes well.
- **Embedding model.** MiniLM is the safe choice but is it strong enough on poetic / figurative text? Run a small evaluation on real entries.

## Visual

- **Procedural nebula approach.** Shader-based (faster, harder to tune) vs. layered noise sprite (easier, less impressive)? Probably shader; budget time for tuning.
- **Helix rendering.** Custom geometry per edge (heavy) vs. instanced helix segments (efficient)? Latter for far edges, former for close.
- **Person node faces.** For person-nodes that don't have photos yet, what does the constellation-portrait show? Generated abstract symbol? Initials?
- **Color regions.** When the spec says "stars change color as you move," is this a gradient mapped to position, or a per-region semantic mapping (childhood = warm, philosophy = violet)?

## Algorithmic

- **Layout stability.** When a new node is added, how do we keep the existing layout stable? Pin positions and let new nodes settle around them, or accept some shuffling?
- **Top-k size.** Is k=50 enough for a graph of 5000 nodes? At what k do we start losing meaningful connections that should be visualized?
- **Manual edge weighting.** When Chase manually draws an edge, how heavy is its weight relative to embedding similarity? Should it always dominate?

## Content

- **Switzerland diary entry: where is it?** Mentioned as the canonical example but not actually in the codebase yet. Chase needs to provide the real text (or write it fresh) for the seed.
- **Are notepad themes (`fragments`, `mirrors`, etc.) the right initial theme set?** Or should Chase curate from a longer list?
- **Privacy stance.** All ML is local-first by default. Are there workflows where Chase would accept a cloud API call (e.g., a one-time deep analysis pass with Claude/GPT)?

## Product / UX

- **MVP entry input format.** Plain textbox? Markdown editor (TipTap, ProseMirror)? Something more contemplative?
- **Confirmation flow for proposed connections.** Modal? Inline cards in the entry panel? Drag-and-drop?
- **Mobile.** Defer entirely, or try to make a graceful read-only mobile view?

## Process

- **Where is Chase keeping the actual writing day-to-day right now?** Notepad app, paper, Obsidian? Important for the import path.
- **How often does Chase want to add entries?** Influences the friction tolerance of the entry flow.
