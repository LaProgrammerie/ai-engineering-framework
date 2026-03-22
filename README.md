# AI Engineering Framework

**Product entry point** for a two-layer, spec-driven workflow around [Kiro](https://kiro.dev), Cursor, and compatible agents.

This repository is **not** a third template. It is an **umbrella**: system view, onboarding, and pointers to the two repositories that carry the actual artefacts.

| Repository | Role |
|------------|------|
| [**ai-engineering-core**](https://github.com/LaProgrammerie/ai-engineering-core) | **Global layer** — steering + reusable skills → synced to `~/.kiro/` |
| [**ai-engineering-template**](https://github.com/LaProgrammerie/ai-engineering-template) | **Project layer** — per-repo canon (`docs/ai/`), Kiro specs, handoff, IDE rules |

---

## Overview

The framework splits concerns:

1. **Global layer** — how agents should behave everywhere: language, scope discipline, review/debug/refactor workflows, and **context-sync** when spec projections drift.
2. **Project layer** — what you are building: product/architecture/standards canon, native Kiro specs, a **cross-tool summary** (`current-spec.md`), and an **execution contract** (`handoff.md`) for implementation sessions.

Agents and tools read **both**: `~/.kiro` supplies portable habits; the repo supplies truth for *this* product.

> **In three lines:** Spec defines intent. Handoff defines execution. Code must follow the handoff.  
> (Stated in [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core) — repeated here because it is the mental model for the whole system.)

---

## Why this exists

AI-assisted development often suffers from:

- context split across IDE, spec tool, and Git host;
- implementation that diverges from the agreed spec;
- vague or expanding scope mid-session.

This framework aims for **predictable**, **shareable**, and **maintainable** collaboration with agents by making **intent** (spec), **visibility** (`current-spec.md`), and **execution scope** (`handoff.md`) explicit, backed by **global** procedural skills (planning, review, debug, etc.).

It is **progressive**: you can adopt pieces, but the design assumes you eventually use both layers together. Deep rationale and file-level detail live in the template — see [Why this template exists](https://github.com/LaProgrammerie/ai-engineering-template#why-this-template-exists).

---

## Architecture

### Layers

| Layer | Location | Owns |
|-------|----------|------|
| **Global** | [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core) → `~/.kiro/steering`, `~/.kiro/skills` | Cross-repo principles, output format, collaboration norms, reusable skills (`planning`, `code-review`, `debugging`, `refactor`, `release-checklist`, `context-sync`, …) |
| **Project** | [ai-engineering-template](https://github.com/LaProgrammerie/ai-engineering-template) (copied into each product repo) | `AGENTS.md`, `docs/ai/*`, `.kiro/specs/`, `.kiro/steering/` (project-specific), `.kiro/skills/` (e.g. `create-handoff`), `.cursor/`, Copilot instructions |

**Precedence:** project `AGENTS.md` and project `.kiro/` **override** global steering when they conflict (see [ai-engineering-core steering](https://github.com/LaProgrammerie/ai-engineering-core/blob/main/steering/00-index.md)).

### System diagram

```mermaid
flowchart TB
  subgraph global["Global layer (machine)"]
    CORE["ai-engineering-core"]
    KHOME["~/.kiro steering + skills"]
    CORE -->|"sync-to-home.sh"| KHOME
  end
  subgraph project["Project layer (each repo)"]
    TPL["ai-engineering-template"]
    REPO["Your product repository"]
    TPL -.->|bootstrap| REPO
  end
  KHOME --> RUNTIME["Kiro / agents / IDE"]
  RUNTIME --> REPO
  REPO --> CHAIN["spec → current-spec → handoff → code"]
```

A tighter **clone → sync → repo** diagram is maintained in [ai-engineering-core README](https://github.com/LaProgrammerie/ai-engineering-core#how-the-two-halves-connect).

### Runtime workflow (conceptual)

1. **Specify** in Kiro under `.kiro/specs/<feature>/` (requirements, design, tasks).
2. **Project** maintains `docs/ai/active/current-spec.md` as a cross-tool summary when the spec changes materially.
3. **Contract** for the coding session: `docs/ai/active/handoff.md` (scope, allowed files, plan, DoD) — often aided by the repo skill `create-handoff`.
4. **Implement** in Cursor (or similar) from handoff + `docs/ai/03-standards.md` (+ architecture as needed).
5. **Global skills** support planning, review, debug, refactor, release, and **context-sync** to realign spec, projections, and code.

Step-by-step: [How the system works](https://github.com/LaProgrammerie/ai-engineering-template#2-how-the-system-works) in the template README.

---

## Repositories (where to go next)

| Need | Go to |
|------|--------|
| Install global steering/skills, skill reference, `sync-to-home.sh` | [**ai-engineering-core**](https://github.com/LaProgrammerie/ai-engineering-core) |
| Bootstrap a new product repo, `AGENTS.md`, `docs/ai`, hooks, “what to update when” | [**ai-engineering-template**](https://github.com/LaProgrammerie/ai-engineering-template) |
| Map of every file’s role and anti-drift rules | [context-map.md](https://github.com/LaProgrammerie/ai-engineering-template/blob/main/docs/ai/context-map.md) (in the template) |

---

## Spec, `current-spec`, handoff, and code

| Artefact | Role |
|----------|------|
| **`.kiro/specs/<feature>/`** | **Native spec** — authoritative requirements / design / tasks in Kiro. |
| **`docs/ai/active/current-spec.md`** | **Projection** — short, tool-agnostic summary so IDE and other agents see the same “active story” without opening every spec file. |
| **`docs/ai/active/handoff.md`** | **Execution contract** — what to build *now*, in which files, with what done means. |
| **Code + tests** | Must align with **handoff**; if spec moved, refresh handoff and/or run **context-sync** (global skill in core). |

Canonical table: [Key file roles](https://github.com/LaProgrammerie/ai-engineering-template#2-how-the-system-works) in the template.

---

## End-to-end workflow (summary)

1. **Plan** (global **`planning`** skill) to structure work before or alongside spec authoring.
2. **Author / update** Kiro spec in the **project** repo.
3. **Sync** `current-spec.md` when the narrative for other tools should change.
4. **Write / refresh** `handoff.md` (project **`create-handoff`** skill).
5. **Implement** from handoff.
6. **Review** with global **`code-review`**; **debug** with **`debugging`**; before release **`release-checklist`**.
7. If things feel out of sync, run **`context-sync`** (global).

Narrative example (login feature, cross-repo): [flow-login.md](https://github.com/LaProgrammerie/ai-engineering-core/blob/main/examples/flow-login.md).

---

## Quickstart

1. **Global layer** — clone [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core), run `./sync-to-home.sh`, reload Kiro.  
   Details: [Install](https://github.com/LaProgrammerie/ai-engineering-core#install-2-minutes).
2. **Project layer** — create a repo from [ai-engineering-template](https://github.com/LaProgrammerie/ai-engineering-template) (fork or use as template), then follow [After cloning this template](https://github.com/LaProgrammerie/ai-engineering-template#after-cloning-this-template-do-this-first).
3. **First feature** — follow the template’s recommended order: spec → `current-spec` → handoff → code (see [§4 What to update when](https://github.com/LaProgrammerie/ai-engineering-template#4-what-to-update-when)).

---

## When to update which layer

**Rule of thumb**

- Change **global** when a habit or workflow should apply to **all** your repos (e.g. how you want reviews formatted, or a new global skill).
- Change **project** when something is **specific to this product** (stack, boundaries, active feature spec, handoff for today’s session).

The template maintains the authoritative **“if you change X, update Y”** table: [What to update when](https://github.com/LaProgrammerie/ai-engineering-template#4-what-to-update-when).

---

## Anti-drift guarantees (what the framework enforces)

Not magic — **conventions + explicit files**:

- **Single source of truth per concern** in `docs/ai/` (product, architecture, standards, workflows, decisions).
- **Spec → projection → handoff** chain so intent is not only inside Kiro.
- **Global skills** (`context-sync`, `debugging`, `code-review`) that are trained to flag **spec vs code** and **stale handoff**.
- **Repo wins over global** where they conflict, so the product layer stays sovereign.

Full map and rules: [context-map.md](https://github.com/LaProgrammerie/ai-engineering-template/blob/main/docs/ai/context-map.md).

**Honest limit:** without discipline (skipping handoff updates, never syncing `current-spec.md`), the system degrades like any process. **Discipline is the product** (see [ai-engineering-core — Why use it](https://github.com/LaProgrammerie/ai-engineering-core#why-use-it)).

---

## Examples and diagrams

| Example | Where |
|---------|--------|
| End-to-end spec → handoff → code (concrete files) | [ai-engineering-template: `examples/simple-feature/`](https://github.com/LaProgrammerie/ai-engineering-template/tree/main/examples/simple-feature) |
| Global + project story (login) | [ai-engineering-core: `examples/flow-login.md`](https://github.com/LaProgrammerie/ai-engineering-core/blob/main/examples/flow-login.md) |
| ASCII flow (spec pipeline) | [template README diagram](https://github.com/LaProgrammerie/ai-engineering-template#diagram-end-to-end-flow) |

Local index for this umbrella repo: [`examples/README.md`](examples/README.md).

---

## Scope of this umbrella repository

**In scope here:** orientation, links, high-level architecture, quickstart pointers.

**Out of scope:** duplicating full steering text, skill bodies, or the template’s `docs/ai` canon — those stay in **core** and **template** respectively.

**Future (optional):** roadmap, ADRs that span both repos, or shared diagrams — can live under `docs/` as the framework evolves.

---

## License

This umbrella repo is released under the [MIT License](LICENSE) (same spirit as the sibling repositories).

---

## GitHub repository metadata (suggested)

- **Description:** `Umbrella: AI engineering framework — global Kiro layer (ai-engineering-core) + project template (ai-engineering-template). Spec → handoff → code.`
- **Topics:** `ai`, `kiro`, `cursor`, `context-engineering`, `spec-driven-development`, `developer-tools`, `workflow`, `documentation`, `agents`
