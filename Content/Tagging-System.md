# Tagging System

> Part of [[Content]]

How tags work, how they're governed, and why discipline here matters.

## The Risk

Tag systems collapse under their own weight. Without governance you end up with:
- 47 near-synonyms ("family," "fam," "relatives," "kin")
- Tags used once and forgotten
- Tag soup that's actively harder to navigate than no tags

## The Approach

### Two Categories of Tags

**1. Themes (`themes`)** — High-significance, intentional tags. The vocabulary of Chase's interior life. Examples: `fragments`, `mirrors`, `family`, `endings`, `authors`, `home`. Maintained as a curated list. Adding a new theme requires explicit confirmation. **These drive spatial layout strongly.**

**2. Casual tags (`tags`)** — Lower-significance, more abundant. Things like specific place names ("Switzerland," "Boston"), specific people ("Mom," "Dad"), generic categorizers ("dream," "poem-fragment"). Auto-suggested freely; less governance overhead. **Weaker layout signal.**

### Tag Storage

In the data model (see [[Architecture/Data-Model]]):
- `tags` — flat string array on every node, casual tags
- `themes` — separate field on entries, references the curated theme registry

Themes themselves are nodes (`type: idea` or `type: symbol`). The theme registry IS a sub-graph of the nexus. This is what lets the user navigate *to* a theme and see all entries connected to it.

## Tag Lifecycle

### Auto-Proposed
The poetry analyzer (see [[Algorithms/Poetry-Analysis]]) proposes tags from new entries. Auto-tags are *suggestions*, not commitments — they're shown to Chase for confirmation.

### Confirmed
Chase confirms a suggestion. Tag is now associated with the node, and its global count increments.

### Manual
Chase types a tag himself. Same as confirmed.

### Promoted to Theme
When a casual tag has been used N times (e.g., 5+) AND has been used across multiple emotional registers / contexts, the system suggests promoting it to a theme.

### Pruned
A tag with very low use count and no recent activity can be flagged for pruning.

## Naming Discipline

- **Lowercase, hyphenated**: `family-of-origin`, `endings`, `poem-fragment`
- **Singular form**: `fragment`, not `fragments` (unless the plural is meaningfully different)
- **Chase's voice**: prefer Chase's own words from his entries over generic dictionary terms
- **No mega-tags**: avoid "deep" or "important" — these don't say anything

## Visualization

- Themes appear as `idea` / `symbol` nodes in the nexus, often densely connected
- Casual tags don't render as nodes by default (they're metadata) but the user can filter the graph by tag and see all nodes with that tag highlighted
- A "theme constellation" view zooms to a theme node and shows the constellation of entries connected to it

## Initial Theme Seeds

From the original notepad, the following themes are already implied:
- `fragments`
- `mirrors`
- `authors`
- `characters`
- `family`
- `memory`
- `endings`
- `identity`

These can be the starting theme registry when [[Seed-Entries]] are loaded.
