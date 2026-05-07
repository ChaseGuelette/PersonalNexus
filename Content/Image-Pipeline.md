# Image Pipeline

> Part of [[Content]]

How a photo becomes a photo-node with the right connections.

## User Flows

### A. Drop a photo into Assets, manually link it
1. Chase puts the file in `media/photos/`
2. Opens the app, sees a "new media" inbox
3. Manually attaches to an existing entry or creates a new photo-node
4. Optionally tags people, location, themes

### B. Bulk upload (e.g., from a phone export)
1. Chase points the app at a folder of photos
2. System imports each photo as a photo-node
3. Pipeline proposes tags + connections for review
4. Chase confirms in batches

## Pipeline Stages

### 1. Ingest
- Copy/move file to `media/photos/<yyyy>/<mm>/`
- Read EXIF: timestamp, GPS, camera info
- Create node with `type: photo`, `media_path` set

### 2. Generate Thumbnails
- Small (256px) for graph display
- Medium (1024px) for entry-panel viewing
- Keep original for full-resolution view

### 3. CLIP Embedding (see [[Algorithms/Embeddings]])
- Compute image embedding via CLIP
- Store in vector DB

### 4. Visual Analysis
- **Face detection** → bounding boxes, optional identity if Chase has confirmed people before
- **Scene classification** → "mountain," "indoor," "beach," "portrait," etc.
- **OCR** → if text appears in the image, extract it (signs, letters, etc.)

### 5. Connection Proposals
- Photos with similar CLIP embeddings → propose visual-similarity connection
- Photos with the same detected faces → propose person connections
- Photos with EXIF location matching a place-node → propose location edge
- Photos taken near the time of an existing entry → propose temporal connection

### 6. User Review
- Same review/confirm flow as text entries

## Face Identity Handling

The first time a face is detected, the user can label it. The label associates that face with a `person` node. Future photos with the same face auto-suggest a connection to that person.

**Privacy note**: This data lives only on Chase's machine. No cloud face recognition.

## Multi-Person Photos

A photo with Chase + Mom + Mom's parents creates connections:
- photo → person:Chase
- photo → person:Mom
- photo → person:Mom's-parents

That photo becomes a powerful hub — any of those four people can navigate to it.

## Performance
- Thumbnail generation: ~50ms per photo (sharp / native libs)
- CLIP embedding: ~200ms per photo on CPU, much faster on GPU
- Face detection: ~150ms per photo
- For bulk upload: batch and queue, run in background worker

## Open Questions
- Albums? Group of related photos as a "moment" — distinct from individual photo-nodes?
- Video frames vs. photos — share schema or separate?
- Auto-rotation/cropping based on face/composition detection?
