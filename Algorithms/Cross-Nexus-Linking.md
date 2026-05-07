# Cross-Nexus Linking

> Part of [[Algorithms]]

> "I've toyed with the idea of the chase nexus being able to connect to the 'Mom' nexus, which I think is how it works. ... I would be close to my mom, she would be close to her parents, and I would be a little further from them cause their my grandparents." — original notepad

How multiple nexuses coexist and connect.

## The Vision

Each person has their own nexus — a self-contained graph. But people relate to each other, and those relationships have structure:
- Chase ↔ Mom (close)
- Mom ↔ Mom's parents (close)
- Chase ↔ Mom's parents (medium — "his grandparents")

The system represents this with **nested or adjacent nexuses** that have controlled connection points between them.

## Two Possible Architectures

### A. Single Federated Graph
All nodes from all nexuses live in one giant graph. Each node has an `owner` attribute. Visualization filters/colors by owner. Spatial layout naturally places "Chase's Mom" near both Chase and Mom's other anchors.

**Pros**: Simple data model. Cross-nexus relations are just edges.
**Cons**: Privacy / ownership boundaries are blurred. If Mom's nexus is sensitive, harder to scope access.

### B. Linked Nexus Graphs
Each nexus is its own graph (its own database). Each has a small set of **boundary nodes** (representations of people known to connect to other nexuses). Boundary nodes carry a pointer (`nexus_link`) to the corresponding node in the other nexus.

**Pros**: Clean ownership boundaries. Possible to share or hide individual nexuses. More mentally aligned with "each person has their own nexus."
**Cons**: More plumbing. Joining/visualizing multi-nexus views is harder.

**Initial decision**: Architecture B. The aesthetic vision (each person's nexus has its own character) maps better. Implement single-nexus first, design boundary-node hooks early.

## Boundary Nodes

When Chase's nexus has a "Mom" person-node, that node is a *boundary*:
- It exists in Chase's nexus, with all the connections Chase has tied to it
- It carries a `nexus_link: <uuid_of_mom_nexus>` field
- When the user opens Mom's nexus, they see *Mom's own self-node* (which is the canonical "Mom") with all the connections Mom would have

The Mom-as-Chase-knows-her node and the Mom-as-Mom-is node are *not the same node*. They're two related entities. This matters: Chase's Mom is a representation; the actual Mom-nexus is its own thing.

## Visualization

When viewing only Chase's nexus, boundary nodes look like other person-nodes (constellation portraits) but glow differently — a halo color indicating "you can travel from here to another nexus."

Selecting a boundary node and choosing "enter" smoothly transitions the camera into the linked nexus. The user feels like they walked through a gateway between two galaxies.

In an "all nexuses" overview view, both galaxies are visible together, with the bridge-helix connections between them shown as larger, more luminous structures than internal connections.

## Spatial Distance

> "I would be a little further from them [grandparents] cause their my grandparents."

Within Chase's nexus, the boundary node "Mom's parents" might exist as a single node with weaker connections to Chase than "Mom" has. The distance reflects relational warmth from Chase's perspective.

In the meta-view, Mom's parents are *anchors of their own potential nexus*, sitting between Mom and Chase but closer to Mom.

## Privacy / Sharing (Future)

Nexuses are inherently personal. If Chase's Mom ever chooses to maintain her own nexus, the question becomes: how do they sync their boundary nodes? Manual confirmation? Selective sharing of certain edges?

Defer this. Build for one nexus first; design boundary-node hooks so adding more later is straightforward.

## Open Questions
- Do we let Chase author "what I think Mom's nexus looks like" (a fictional/inferred Mom-nexus from his perspective) even before Mom has her own?
- How do boundary edges weight in the association matrix — same as internal edges, or different?
- What's the spatial layout algorithm when multiple nexuses are visible together?
