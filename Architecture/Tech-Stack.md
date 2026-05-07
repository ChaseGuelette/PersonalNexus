# Tech Stack

> Part of [[Architecture]]

The full kit. Provisional for the MVP — things will shift.

## Frontend
- **Vite** (build tool, dev server)
- **React 18+** (component framework)
- **TypeScript** (type safety)
- **Three.js + React Three Fiber + drei + postprocessing** (3D — see [[3D-Engine]])
- **Zustand** or **Jotai** (lightweight state for graph/UI; Redux is overkill)
- **Tailwind** (utility CSS for the 2D UI surfaces — entry panels, modals)

## Backend / Data Layer
- **Node.js or Python** (whichever runs the embedding pipeline; Python has better ML ecosystem)
- **SQLite** (primary store)
- **sqlite-vec** OR **ruvector.db** (vector store — pending inspection of existing ruvector.db)
- Optional: **Tauri** if we want a native desktop wrapper later (lighter than Electron)

## ML / Embeddings
- **sentence-transformers** (`all-MiniLM-L6-v2`) for text — local, small, free
- **CLIP** (`openai/clip-vit-base-patch32`) for image embeddings
- **Whisper** for video transcripts
- **OpenAI** or **Anthropic** API for poetry-style theme analysis (if local LLMs aren't strong enough yet)

## Build / Dev Tooling
- **OpenCode + GLM** for code implementation (already configured — see [[CLAUDE.md]])
- **pnpm** or **npm** for packages
- **Vitest** for tests
- **Playwright** for end-to-end (eventually)

## Deployment
- TBD. Single-user local-first means no urgent server needs. Could be:
  - Static hosted frontend + local SQLite (Tauri desktop)
  - Or a lightweight Node/Python backend if we need server-side embedding compute

## What's NOT in the Stack
- Heavy frontend frameworks (Next.js, Remix, Angular)
- Backend frameworks (Express alone is fine; no Nest/Fastify needed yet)
- Cloud storage / databases (Postgres, Mongo) — overkill for one user
- React Native / mobile (web-first)
