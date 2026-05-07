# Poetry Analysis

> Part of [[Algorithms]]

> "This poetry style analysis where I can look at the things I've written about and then analyse them. Attach tags and connection points to other points and things inside the matrix." — original notepad

The system that reads an entry the way a literary critic would: identifying themes, symbols, recurring motifs, emotional registers — and proposing connections to existing nodes in the graph.

## The Switzerland Example

> "I wrote a diary entry about my Switzerland trip. It discussed my feelings about the trip, about my family, about my emotions, and then deviated into a mini poem about authors and how the characters they write are fragments of themselves... So the diary entry would be there, and it would be connected to the idea of authors, characters, mirrors, fragments, family, memories, and then it would be connected to images of of the trip itself."

That's the gold standard. From one entry, the system should extract:
- **Place**: Switzerland
- **People**: family (specifically Mom if mentioned)
- **Emotions**: whatever the entry conveys
- **Themes**: authors, characters, mirrors, fragments, identity
- **Symbols**: characters-as-fragments (a recurring symbol)
- **Connected media**: photos of the trip
- **Connected entries**: any other entries about authors, fragments, family, etc.

## Pipeline

### Stage 1: Surface Tags (cheap, automatic)
- Named-entity extraction (people, places, dates) → SpaCy or similar
- Keyword extraction → KeyBERT or YAKE
- Sentiment / emotion → a small classifier or LLM call

### Stage 2: Theme Extraction (LLM-powered)
Send the entry + the existing theme taxonomy to an LLM with a prompt like:

> Given this entry, identify:
> 1. Existing themes from the taxonomy that are present (with confidence)
> 2. Any new themes worth proposing for the taxonomy
> 3. Specific symbols or motifs that recur or might recur
> 4. Emotional register

Return structured JSON.

### Stage 3: Symbol/Motif Detection
For specific recurring images Chase writes about (e.g., "mirrors," "fragments," "endings," "doors"), maintain a symbol registry. New entries are checked against it. New symbols can be added, growing the personal symbol-vocabulary over time.

### Stage 4: Connection Proposal
Combine outputs of stages 1-3 with the embedding-similarity top-k from [[Association-Matrix]]. Propose:
- "This entry connects to X (because both share theme `fragments`)"
- "This entry resembles Y (high embedding similarity)"
- "This entry mentions Z, who is a person-node"

Show proposals to Chase. Confirmation upgrades them to manual edges.

## Why LLM, Not Just Embeddings

Embeddings give similarity, not interpretation. The Switzerland entry's poetic detour about authors and fragments is the *interesting* part — it links the diary to a whole region of the graph that pure cosine similarity might miss because the surface text is mostly about a trip.

An LLM can read the digression and say: "this is also about identity and self-fragmentation." That's the leap embeddings can't make alone.

## Local vs. Remote LLM

For privacy (intimate journal data), prefer a local model. Try:
- **Llama 3.1 / 3.2** (8B or 13B, quantized, via Ollama)
- **Phi-3.5**
- **Mistral**

Fall back to Anthropic / OpenAI API only if local quality is meaningfully worse on Chase's actual entries.

## Tag Taxonomy Discipline

Theme/tag explosion is a real risk. (See [[Content/Tagging-System]] for governance.) The poetry analyzer should propose new tags but not auto-create them; Chase confirms. This keeps the vocabulary feeling like *his* language rather than ML noise.

## Open Questions
- How much "interpretation" is too much? At what point does the analyzer start telling Chase what his entries *mean* in a way that overrides his own reading?
- Can we have the analyzer cite its evidence (line numbers in the entry) for every theme it proposes?
- Periodic re-analysis: as the symbol vocabulary grows, should old entries be re-analyzed against the new vocabulary?
