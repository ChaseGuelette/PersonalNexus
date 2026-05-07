# Storage

> Part of [[Architecture]]

Where data lives and how we get it back.

## What's Already on Disk

- `ruvector.db` (~1.5MB at project root) — vector database. Inherited; will be used as the embedding store. Need to inspect schema.
- `images/` — current home of visual references. Will eventually also hold user-uploaded photos (or that gets a separate `media/` folder).

## Proposed Storage Layout

### Primary Store: SQLite + sqlite-vec (or DuckDB)
For a single-user local-first app, SQLite is the right primitive. Use the `sqlite-vec` extension for vector similarity, OR keep `ruvector.db` for vectors and a separate `nexus.db` for the graph — depends on what `ruvector.db` actually is.

**Tables (in `nexus.db`)**:
- `nodes` (id, type, created_at, updated_at, title, body, media_path, tags JSON, metadata JSON, position_x, position_y, position_z)
- `edges` (id, source_id, target_id, type, weight, origin, created_at, metadata JSON)
- `tags` (id, name, count) — denormalized for fast filtering
- `embeddings` (node_id, vector BLOB, model_name, computed_at) — OR live in `ruvector.db`

### Media Storage
- `media/photos/` — user-uploaded photos (not the moodboard images)
- `media/videos/` — uploaded video files
- `media/thumbnails/` — generated thumbnails

Reference media by relative path stored in `nodes.media_path`. Never hardcode absolute paths.

### Indexes
- `idx_nodes_type` on `nodes(type)`
- `idx_edges_source` on `edges(source_id)`
- `idx_edges_target` on `edges(target_id)`
- Vector index on embeddings (HNSW via sqlite-vec or via ruvector)

## Action Items (early)
1. **Inspect `ruvector.db`** — what's its schema, what data is already there. ([[builder]] task)
2. Decide: extend `ruvector.db` or pair it with a new `nexus.db`.
3. Pick an embedding model (likely `all-MiniLM-L6-v2` for text — small, local — or OpenAI `text-embedding-3-small` if we go remote). See [[Algorithms/Embeddings]].

## Backup and Versioning
- Database lives in the project root for now (single-user, no cloud).
- Eventually: nightly snapshot to `backups/YYYYMMDD/`.
- Long-term: optional encrypted cloud backup (this is intimate data).
