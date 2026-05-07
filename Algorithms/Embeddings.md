# Embeddings

> Part of [[Algorithms]]

How we turn entries, photos, and ideas into vectors so we can compute similarity.

## Text Embeddings

### Model Choice

**Default**: `sentence-transformers/all-MiniLM-L6-v2`
- 384 dimensions
- ~80MB model file, runs locally
- Strong on short-to-medium text
- Free, no API calls

**Alternative**: `text-embedding-3-small` (OpenAI)
- 1536 dimensions
- Better quality, especially on poetic/figurative text
- Costs $$ and requires network
- Use if MiniLM proves too weak on Chase's literary entries

**Why local first**: This is intimate data. Send it over the wire only if absolutely necessary. (See [[Vision/Non-Goals]].)

### Pipeline

For each text node:
1. Take `body` text (or `body` + `title`)
2. Strip markdown / formatting
3. Pass through embedding model
4. Store as `float[384]` (or 1536) in vector DB
5. Store `model_name` alongside so we can detect when re-embedding is needed after a model change

### Re-embedding
- Whenever `body` is edited
- When the model is upgraded (rare)

## Image Embeddings

**Default**: CLIP (`openai/clip-vit-base-patch32`)
- Joint text-image embedding space
- Lets us search photos with text queries ("find photos of mountains")
- Lets us cluster photos by visual similarity

### Pipeline
1. Load image
2. Resize to 224×224
3. Pass through CLIP image encoder
4. Store vector

## Video Embeddings

For v1, treat videos as:
- Audio → Whisper transcript → text embedding (for the transcript)
- Visual → keyframe extraction (every N seconds) → CLIP image embedding for each
- Composite: average or attention-weighted combination

This is the most expensive pipeline. Defer until photos and text work end-to-end.

## What `ruvector.db` Already Is

**Pending inspection.** First [[builder]] task in this area: open `ruvector.db`, document its schema, decide whether to extend it or pair it with a new vector store.

## Quality Checks

Before committing to a model, run a small evaluation:
- Embed Chase's 5-10 most distinct entries
- Compute pairwise similarity
- Verify intuitive groupings (e.g., the Switzerland diary should cluster with other "trip" entries, not with random poems)

If the model fails this, switch.

## Open Questions
- Hybrid embedding (text + structured tags) — does that give better clustering?
- How to handle very short notes (a few words)? Embeddings get unstable below ~10 tokens.
- For "ideas" (e.g., "fragments," "mirrors") — should they be embedded as their definition, or as a synthetic description Chase writes?
