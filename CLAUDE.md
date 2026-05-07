# PersonalNexus - Agent Instructions

> Part of [[BRAIN-INDEX]]

## What Is This Brain?

PersonalNexus is a 3D interactive knowledge graph — somewhere between a journal, a notepad, and a museum of self — that visualizes Chase's thoughts, memories, and creative fragments as a living artwork. Entries (text, images, video) become nodes; relationships become DNA-helix-style connections; the whole thing lives in 3D space where related ideas sit physically close. The aesthetic is cosmic: deep blue nebulas, sacred geometry, ticking watches in space, Skyrim-skill-tree navigation.

It is NOT a productivity tool. It is a way to make sense of, and witness, the flurry of things in Chase's head.

## Owner
- **Role**: Founder & Solo Developer
- **Context**: Early-stage build. Vision is articulated (see [[Vision/Core-Concept]]). Tooling is set up: OpenCode + GLM workflow, vector DB (`ruvector.db`) at root, brain-tree OS viewer running on port 3000. No application code yet — first task is scaffolding the data model and a minimal 3D viewer.
- **Goals**:
  1. Get a minimum viable 3D nexus on screen — orb + helixes + a handful of seed nodes — that proves the visual concept
  2. Build the ingestion pipeline so journal entries, images, and videos can flow in
  3. Develop the association/embedding algorithm that places nodes meaningfully in 3D space
  4. Keep the artistic vision uncompromised — this is art, not just software

## Brain Structure
- [[Vision]] - Core concept, experience goals, inspirations, success criteria
- [[Design]] - Visual language, backgrounds, node aesthetics, navigation model
- [[Architecture]] - Data model, storage, 3D engine choice, performance strategy
- [[Algorithms]] - Association matrices, embeddings, 3D layout, poetry analysis
- [[Content]] - Ingestion pipelines (text/image/video), tagging, seed entries
- [[Build]] - MVP scope, phased milestones, open questions
- [[Assets]] - Pointers into the project's `images/` reference folder
- [[Handoffs]] - Session continuity notes
- [[Templates]] - Reusable note structures

## Conventions
- Use [[wikilinks]] for all cross-references between notes, but ONLY link to files that exist. Never create wikilinks to files that haven't been created yet.
- Keep files concise and actionable
- Tag files with relevant hashtags for discoverability
- Check [[Assets]] for related images, videos, PDFs when working on any task
- Update Handoffs/ at the end of every work session
- Reference the [[Execution-Plan]] as the source of truth for build order

## Critical Invariants
These rules must NEVER be violated:
1. **Keep Explore agents narrow** — max_turns: 10, focused prompts, 5-12 tool calls max
2. **After plan approval, HAND OFF to GLM via OpenCode** — do NOT read implementation files into your context. Write the spec, attach source files via `-f` flags, and let GLM do the reading and editing. Fall back to direct implementation ONLY if OpenCode fails (server down, escaping issues, timeout).

## Assets
The [[Assets]] folder contains images, videos, PDFs, and other media. When working on any task, check Assets/ for related materials. You can analyze images, read PDFs, and process any file dropped there.

## Model Routing

The user manually selects the session model (`/model opus`, `/model sonnet`). No auto-escalation rules — the model in use handles everything directly.

For mechanical subtasks, delegate to cheaper subagents:
- **Haiku**: File search (explorer)
- **Sonnet**: Brain file I/O (resume/wrap-up braintree skills)

### Code Implementation via OpenCode + GLM (PREFERRED — fallback to direct edit)
A headless OpenCode server runs at `http://localhost:4096` (started from the project root).

**RULE: When a plan is approved, IMMEDIATELY hand off implementation to GLM via OpenCode.** Do NOT read implementation files into your context first — you already have the plan. Write the spec file, attach the target source files via `-f` flags (GLM reads them, not you), and hand off. If OpenCode fails (server down, escaping issues, timeout, file-not-found errors), fall back to implementing directly. Do not retry OpenCode more than once — if it fails, just do the work yourself.

**This applies when ANY of these are true:**
- The total estimated edits exceed ~20 lines of changes (even in a single file)
- The plan touches 2+ code files
- A plan or spec exists with clear file paths, function names, and behavior AND the changes touch code

**CRITICAL: Do NOT read implementation files into your own context after plan approval.** The plan describes what to change. The `-f` flag gives GLM the source files as context. Reading 1000+ line files into your context and then implementing directly defeats the purpose of GLM delegation and wastes tokens.

**Workflow:**
1. Write the spec to a file, then pass it to OpenCode. **ALWAYS use `run_in_background: true`** on the Bash tool — GLM calls can exceed the 2-minute timeout.
   ```bash
   # CRITICAL: Prompt MUST come BEFORE -f flags. OpenCode's -f is a greedy array
   # that swallows subsequent positional args as filenames. Wrong order = "File not found" error.

   # For short specs (< 5 lines): inline as the prompt argument
   export OPENROUTER_API_KEY=$(grep OPENROUTER_API_KEY .env | cut -d'"' -f2) && opencode run --attach http://localhost:4096 -m openrouter/z-ai/glm-5.1 "short spec here"

   # For long specs (> 5 lines): write to file first, then attach with -f
   # Step A: Generate a unique spec filename: .claude/glm_spec_<timestamp>.md
   #         Example: .claude/glm_spec_20260420_143052.md
   #         Use: SPEC_FILE=".claude/glm_spec_$(date +%Y%m%d_%H%M%S).md"
   # Step B: Use the Write tool to create the spec file with the full plan
   # Step C: Run OpenCode — PROMPT FIRST, then -f flags
   SPEC_FILE=".claude/glm_spec_$(date +%Y%m%d_%H%M%S).md"
   export OPENROUTER_API_KEY=$(grep OPENROUTER_API_KEY .env | cut -d'"' -f2) && opencode run --attach http://localhost:4096 -m openrouter/z-ai/glm-5.1 "Implement the spec in the attached file. The other attached files are existing code for context." -f "$SPEC_FILE" -f src/target_file.py
   ```
   **ARGUMENT ORDER**: `"prompt text" -f file1 -f file2` — NEVER `-f file1 "prompt"` (prompt gets eaten as a filename).
   **Use relative paths** in specs (not absolute) — the server runs from the project root.
   **Spec filenames must be unique** — use `.claude/glm_spec_<YYYYMMDD_HHMMSS>.md` (not a fixed name). Multiple sessions can run concurrently.
   The `-f` flag attaches files for GLM to read as context (existing source files AND the spec file).
2. **Review GLM's work (diff-only — do NOT read full files).** Check the background task output with `TaskOutput`, then run `git diff -- <file1> <file2> ...` scoped to ONLY the files listed in your spec. Do NOT use bare `git diff` — other terminals may have concurrent uncommitted changes that would pollute the review. You already have the plan in your context — compare the diff against it. Do NOT Read the modified files; the diff is sufficient. Check for:
   - Missing steps from the plan (GLM skipped something)
   - Wrong imports or function signatures
   - Logic that doesn't match the spec
   - Project conventions violated
   If the diff matches the plan: done. If mismatches found: flag them to the user with specifics ("GLM missed step 3" or "wrong function signature on line X").
3. Fix small issues directly (Edit tool). For larger problems, send corrections back to OpenCode with a targeted prompt.
4. Run tests/linting if applicable.

**Model tiers:** `z-ai/glm-5.1` (default — best quality, $0.95/$3.15), `z-ai/glm-4.5-air:free` (fallback — FREE, simpler tasks)

**If the server is not running**, start it: `cd /c/Users/Chase/Projects/PersonalNexus && export OPENROUTER_API_KEY=$(grep OPENROUTER_API_KEY .env | cut -d'"' -f2) && opencode serve --port 4096`

**Known failure modes (fall back to direct edit if any occur):**
- "File not found: <prompt text>" — Prompt is AFTER `-f` flags. Fix: put prompt BEFORE all `-f` flags (`"prompt" -f file`, not `-f file "prompt"`).
- `/tmp/` writes fail — Windows doesn't have `/tmp/`. Use `.claude/glm_spec_<timestamp>.md` instead.
- Bash tool returns "Error: Exit code 1" repeatedly — Bash may be non-functional in the session. Fall back to direct Edit.
- Concurrent OpenCode calls can collide — send one at a time.
- **Concurrent terminal diffs collide** — When multiple terminals delegate to GLM, bare `git diff` shows ALL terminals' changes. Always scope: `git diff -- file1.py file2.py` using only the files from your spec.
- **Exit code 1 ≠ GLM failed** — OpenCode transport can return exit code 1 even when GLM successfully wrote all files. Before falling back, check whether the target output file(s) exist and have content. If GLM wrote them, skip the fallback to avoid duplicate work. Only fall back if the files are missing or empty.

**Do NOT use OpenCode for:** small edits (< 20 lines), config changes, brain/markdown updates, or tasks requiring deep cross-system reasoning.

### Context Protection Rules
- **Keep Explore agent prompts narrow and bounded.** Bad: "explore the whole codebase". Good: "find the entry point for X in src/Y/ and list its public functions". Set `max_turns: 10` on Explore agents to prevent runaway exploration. A focused Explore should use 5-12 tool calls, not 25+.
- **Explore agent threshold rules:**
  - **1-2 searches**: Use Grep/Glob directly — no Explore agent needed
  - **3+ searches OR reading 3+ files**: Launch an Explore agent
  - **Unknown scope** (don't know which files are relevant): Launch an Explore agent
  - When in plan mode: ALWAYS use Explore agents for investigation
- **Keep Plan agent prompts authoritative and bounded.** Set `max_turns: 15` on all Plan agent Task calls. Include in the prompt: "The context provided above is authoritative — do NOT re-read files already described. Only look up files not covered in this prompt." A Plan agent should design a solution from the context it receives, not re-explore the codebase. If it needs more context, the Explore phase was insufficient — fix the Explore prompts, don't let Plan agents compensate with unbounded exploration.
- **Brain-first exploration.** When investigating a system, start from the BrainTree (`brain/` folder) for orientation before diving into source code. Read the relevant brain doc first to understand architecture, then go to source files for current implementation details.
- **After plan approval, hand off to GLM via OpenCode immediately.** Do NOT read the implementation files yourself — write the spec to `.claude/glm_spec_<timestamp>.md` (unique per session), attach source files via `-f` flags, run with `"prompt" -f <spec_file> -f <source_files>` (prompt BEFORE -f), backgrounded. GLM reads the files, not you. If OpenCode fails, fall back to direct implementation — don't waste tool calls retrying.

## Subagents
Specialized agents in .claude/agents/:
- [[explorer]] - Haiku-powered codebase search and file reading (max 5-12 tool calls)
- [[builder]] - Implementation, 3D rendering, code execution
- [[architect]] - Systems design, data models, algorithm trade-offs
- [[artist]] - Visual direction, aesthetic coherence
- [[curator]] - Content organization, tagging, poetry-style analysis

## Commands
- /init-braintree - Initialize a new brain
- /resume-braintree - Resume from where you left off
- /wrap-up-braintree - End session with proper handoff
- /status-braintree - View progress dashboard
- /plan-braintree [step] - Plan a specific step
- /sprint-braintree - Plan the week's work
- /sync-braintree - Health check and sync
- /feature-braintree [name] - Plan a new feature
