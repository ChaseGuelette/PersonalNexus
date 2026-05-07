# Data Model

> Part of [[Architecture]]

The schema that holds Chase's interior life. Designed to be a property graph — typed nodes, typed edges, attributes everywhere, with embeddings attached for similarity.

## Core Types

### Node (base type)
Every entity in the nexus is a node.
- `id` (uuid)
- `type` (entry | photo | video | person | place | event | idea | symbol)
- `created_at` (timestamp)
- `updated_at` (timestamp)
- `title` (short, optional — used for display)
- `body` (longer text content; not all node types have this)
- `media_path` (relative path to file in `images/` or future `media/` folders)
- `tags` (string[])
- `embedding` (float[384] or float[1536]; stored in `ruvector.db`)
- `position` (vec3 — computed 3D coordinates; recomputed when graph changes)
- `metadata` (jsonb — type-specific attributes)

### Edge (relationship)
- `id` (uuid)
- `source_id` (node uuid)
- `target_id` (node uuid)
- `type` (mentions | depicts | family_of | located_in | references | continues | echoes | manual)
- `weight` (float 0–1 — strength of association)
- `origin` (auto | confirmed | manual) — was this edge auto-suggested, confirmed by Chase, or drawn manually
- `created_at` (timestamp)
- `metadata` (jsonb — context, e.g., "this entry mentions Mom in paragraph 2")

## Type-Specific Metadata

### `type: entry` (journal/notepad text)
- `mood` (optional)
- `themes` (string[]) — e.g., ["fragments", "mirrors", "family"]
- `excerpt` (auto-generated short pull quote)

### `type: person`
- `display_name`
- `relationship` (mom, friend, self, etc.)
- `nexus_link` (uuid of another nexus this person owns/anchors, optional — the cross-nexus topology)

### `type: photo` / `type: video`
- `taken_at`
- `location` (lat/lng or place_id)
- `subjects` (node ids of people detected/tagged in the photo)
- `caption` (auto + manual)

### `type: idea` / `type: symbol`
- `definition` (Chase's own description of what this idea means to him)
- `frequency` (how often it appears — driver of node visual size)

## Invariants (do not violate)

1. **Every edge has both endpoints existing.** No dangling references.
2. **Embeddings are recomputed when body changes.** Stale embeddings cause bad layout.
3. **Position is computed, not stored long-term as canonical.** It can be recomputed from the graph + embeddings; treat it as cache.
4. **Origin is preserved.** Never overwrite a `manual` edge with an `auto` edge — manual wins.

## Open Questions
- Do we need versioned nodes (history of edits)?
- Should `tags` themselves be nodes (allowing tag-of-tag relationships)? Probably yes for "themes," no for casual tags.
- How do we handle the cross-nexus connection without duplicating data?
