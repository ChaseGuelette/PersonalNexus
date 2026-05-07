# Architecture

> Part of [[BRAIN-INDEX]]

How PersonalNexus is built under the hood. Tech stack, data shapes, storage, runtime, and performance strategy.

This folder is the canonical technical reference. When [[builder]] or [[architect]] needs to know "how is X stored / rendered / computed," the answer lives here.

## Key Files
- [[Data-Model]] - Nodes, edges, types, attributes — the schema for everything
- [[Storage]] - How and where data lives (`ruvector.db`, files, indexes)
- [[3D-Engine]] - Rendering stack choice and rationale
- [[Tech-Stack]] - Languages, frameworks, dependencies
- [[Performance-Strategy]] - LOD, culling, lazy loading, frame budget
