# Video Pipeline

> Part of [[Content]]

The most expensive ingestion path. Defer until [[Text-Pipeline]] and [[Image-Pipeline]] are working end-to-end.

## What a Video-Node Is

A video file plus:
- Transcript (if any speech)
- Keyframe samples (representative thumbnails)
- Multi-modal embedding (combination of audio transcript + visual)
- Detected people, places, scenes (per keyframe)

## Pipeline Stages

### 1. Ingest
- Copy file to `media/videos/<yyyy>/<mm>/`
- Read metadata: duration, resolution, codec, EXIF/QuickTime location
- Create node with `type: video`

### 2. Transcribe
- Run audio through Whisper (local)
- Store transcript with timestamps
- Embed transcript text → text embedding

### 3. Sample Frames
- Extract keyframes every N seconds (start with N=2 for short videos, N=10 for longer)
- For each keyframe:
  - Generate thumbnail
  - Run face detection + scene classification
  - Compute CLIP embedding

### 4. Composite Embedding
- Average or attention-weighted combination of:
  - Transcript text embedding
  - Mean/max of keyframe CLIP embeddings
- This is the canonical embedding for similarity search

### 5. Connection Proposals
- Same as image pipeline, plus:
- Transcript text → matches with text-entry themes
- Specific timestamps where a face appears can link to person-nodes

### 6. Review + Persist

## Storage Concerns

Videos are big. For one user this is fine — local disk handles it. But generated keyframe thumbnails plus the original can balloon storage.

**Strategy**:
- Keep originals in `media/videos/`
- Keep keyframe thumbnails in `media/video-frames/<video_id>/`
- Generate web-friendly compressed version for in-app playback (`media/video-web/<video_id>.mp4`)

## Visual Representation

A video-node renders as a small "card" or framed window with the keyframe thumbnail looping (or a brief auto-extracted teaser clip). On focus, the full video plays in the entry panel.

## Performance
- Whisper transcription: ~1x real-time on CPU, faster on GPU
- Keyframe extraction: ~real-time via ffmpeg
- CLIP per frame: 200ms each
- Total for a 5-minute video: ~10-20 minutes of background processing

Run in a background worker queue. Show progress in the UI.

## Open Questions
- For very long videos (interviews, recordings), should the system propose chapters or thematic segments as their own sub-nodes?
- Audio-only files: separate "audio" node type, or absorbed into video?
- Live capture (using device camera/mic to record straight into the nexus)?
