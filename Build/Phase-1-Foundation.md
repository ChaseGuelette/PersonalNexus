# Phase 1: Foundation

> Part of [[Build]]

Get the data layer working before any pixels are drawn. Boring but load-bearing.

## Goals
- Database schema and migrations in place
- Embedding pipeline working (text only, MVP)
- Basic CRUD via a local API or direct DB interface
- Seed loader that imports a few entries
- Test coverage for the data layer

## Deliverables

### 1. Inspect `ruvector.db`
- Open the existing file at project root
- Document its schema in [[Architecture/Storage]]
- Decide: extend it, or pair it with a new `nexus.db` for graph data

### 2. Schema
- `nodes` table per [[Architecture/Data-Model]]
- `edges` table
- Migration script (kept simple — Drizzle / Prisma, OR just plain SQL files)

### 3. Embedding Pipeline
- Install `sentence-transformers` (Python) OR `@xenova/transformers` (Node)
- Function `embed(text) -> float[384]`
- Store in vector store

### 4. Top-K Computation
- Function `top_k_similar(node_id, k=20) -> [(node_id, score)]`
- Performance target: < 100ms for k=20 on a graph of 1000 nodes

### 5. Seed Loader
- Read [[Content/Seed-Entries]] structure
- Insert initial nodes (Chase, Mom, Switzerland, theme nodes, sample entries)
- Compute initial top-k matrix

### 6. Tests
- Unit tests for embedding determinism
- Integration test: seed-load + similarity query
- Snapshot of expected top-k for a known seed

## Done Criteria
- `npm run seed` (or equivalent) populates the DB
- A simple CLI / REPL can query: "what are the top 5 entries similar to entry X?"
- The output makes intuitive sense (not random)
- All tests pass

## Estimated Effort
3-7 days of focused work. Most of the time goes into picking the right libraries and getting embeddings deterministic, not into volume of code.
