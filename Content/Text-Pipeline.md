# Text Pipeline

> Part of [[Content]]

How a journal entry, poem, or note gets from "Chase wrote some words" to "a node in the nexus with all its connections proposed."

## User Story
1. Chase opens a "new entry" view (in the app, eventually — for MVP, it might be paste-into-textbox).
2. Writes the entry.
3. Submits.
4. The system processes it, proposes tags + connections.
5. Chase reviews, adjusts, confirms.
6. The new node appears in the 3D nexus, drifting into its position.

## Pipeline Stages

### 1. Capture
- Markdown text + optional title
- Optional metadata: date written, mood, location

### 2. Persist
- Write the raw entry to `nodes` table with `type: entry`
- File path for any attached media stored in `media_path`

### 3. Embed (see [[Algorithms/Embeddings]])
- Compute text embedding via local sentence-transformer
- Store in vector store, alongside `node_id`

### 4. Surface Analysis (cheap, automatic)
- NER for people / places / dates → propose tags
- Keyword extraction → propose tags
- Sentiment classification → metadata

### 5. Deep Analysis (LLM, see [[Algorithms/Poetry-Analysis]])
- Theme extraction
- Symbol detection
- Connection proposals (this entry is similar to / continues / mirrors entries X, Y, Z)

### 6. User Review
- Show tag and connection proposals to Chase
- Chase confirms / rejects / modifies
- Confirmed connections become edges with `origin: confirmed`

### 7. Position
- Compute initial position from associations (see [[Algorithms/Spatial-Layout]])
- Animate the node into the scene

## Markdown Support

Entries are markdown. Support:
- Headers, lists, blockquotes
- Inline emphasis
- `[[wikilinks]]` to other nodes by title — these become explicit edges with `origin: manual`
- `#tag` syntax for inline tagging
- Embedded images via `![](path)` — automatically promoted to photo-nodes connected to this entry

## Performance
- Embedding: ~50ms for a typical entry on local hardware
- LLM theme analysis: ~2-5s (acceptable, async)
- Spatial reposition: ~100ms for an incremental layout pass

## Edge Cases
- **Very short entries** (< 10 words): may have unstable embeddings. Consider grouping with related entries before embedding, or skipping similarity-based connections and relying only on manual.
- **Very long entries** (> 5000 words): chunk and average, OR use a long-context embedding model. Long entries probably split naturally into multiple thematic sections; consider letting Chase manually split.
- **Edits**: Re-embed, re-analyze, re-propose. Don't silently overwrite confirmed connections — flag conflicts.
