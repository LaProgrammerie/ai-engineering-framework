<p align="center">
  <img src="./assets/logo/logo.png" alt="AI Engineering Framework" width="160" />
</p>


# AI Engineering Framework

**Product entry point** for a two-layer, spec-driven workflow around [Kiro](https://kiro.dev), Cursor, and compatible agents.

This repository is **not** a third template. It is an **umbrella**: system view, onboarding, and pointers to the two repositories that carry the actual artefacts.

| Repository | Role |
|------------|------|
| [**ai-engineering-core**](https://github.com/LaProgrammerie/ai-engineering-core) | **Global layer** — steering + reusable skills → synced to `~/.kiro/` |
| [**ai-engineering-template**](https://github.com/LaProgrammerie/ai-engineering-template) | **Project layer** — per-repo canon (`docs/ai/`), Kiro specs, handoff, IDE rules |

---

## Demo

<p align="center">
  <img src="./docs/demo.gif" alt="Demo: Kiro spec → handoff.md → implementation in Cursor (record per docs/demo-script.md)" width="720" />
</p>

This loop is the framework in one glance: **intent** lives in the Kiro **spec** (requirements / design / tasks), **execution scope** is frozen in **`handoff.md`**, and the IDE implements **only** what the handoff allows—so spec, contract, and code stay aligned.  
Record your own walkthrough with the step-by-step guide: **[`docs/demo-script.md`](docs/demo-script.md)**.

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

### The chain at a glance

```
.kiro/specs/<feature>/  →  docs/ai/active/current-spec.md  →  docs/ai/active/handoff.md  →  code (+ tests)
```

```mermaid
flowchart LR
  S[Spec] --> C[current-spec]
  C --> H[handoff]
  H --> X["code + tests"]
```

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

### A. Ultra quick (copy / paste)

**Once per machine — global layer into `~/.kiro`:**

```bash
git clone https://github.com/LaProgrammerie/ai-engineering-core.git
cd ai-engineering-core
chmod +x sync-to-home.sh && ./sync-to-home.sh
# Restart or reload Kiro
```

**New project from the template:**

```bash
mkdir my-project && cd my-project
git clone https://github.com/LaProgrammerie/ai-engineering-template.git .
```

**Then in Kiro and Cursor:**

```text
Kiro: open folder → my-project
Kiro: create .kiro/specs/first-feature/ (requirements.md, design.md, tasks.md)
Kiro: run skill create-handoff → docs/ai/active/handoff.md
Cursor: open my-project → implement only what handoff.md allows
```

SSH URLs: `git@github.com:LaProgrammerie/ai-engineering-core.git` and `git@github.com:LaProgrammerie/ai-engineering-template.git`.

---

### B. Guided (what each step does)

| Step | What happens |
|------|----------------|
| Clone **ai-engineering-core** + **`./sync-to-home.sh`** | Copies steering + skills into **`~/.kiro/`** so every repo gets the same agent habits. |
| **`mkdir` / clone template into `my-project`** | You get `AGENTS.md`, `docs/ai/`, `.kiro/specs/`, `.cursor/`, and the **`create-handoff`** skill. |
| Create **`.kiro/specs/<feature>/`** in Kiro | Native spec: intent split into requirements → design → tasks. |
| Run **`create-handoff`** | Fills **`docs/ai/active/handoff.md`** — the execution contract for this session. |
| Implement in **Cursor** | IDE follows **handoff** + **`docs/ai/03-standards.md`**; keeps scope bounded. |

When the spec story should be visible outside Kiro, edit **`docs/ai/active/current-spec.md`**. If spec and code diverge, use global **`context-sync`** in Kiro.

---

### C. Conceptual (how it fits together)

The minimum path through the system is: **sync core → bootstrap from template → spec → (optional) current-spec → handoff → code.**  
Progressive adoption is OK; full value shows when both layers and the spec chain are in use.

- **Bootstrap checklist (real product):** [After cloning this template](https://github.com/LaProgrammerie/ai-engineering-template#after-cloning-this-template-do-this-first) — fill `AGENTS.md`, `docs/ai/01–03`, invariants.  
- **Install detail:** [ai-engineering-core — Install](https://github.com/LaProgrammerie/ai-engineering-core#install-2-minutes).  
- **Feature order:** [What to update when](https://github.com/LaProgrammerie/ai-engineering-template#4-what-to-update-when).

---

## First feature in 5 minutes

_No theory — do this in order._

1. **`./sync-to-home.sh`** from a clone of [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core) if you have not already; reload Kiro.
2. **`git clone https://github.com/LaProgrammerie/ai-engineering-template.git try-framework && cd try-framework`**
3. Open **`try-framework`** in **Kiro**. Add **`.kiro/specs/hello/`** with three small files: **`requirements.md`** (one sentence goal), **`design.md`** (one paragraph), **`tasks.md`** (2 bullet tasks).
4. Optionally one-line update **`docs/ai/active/current-spec.md`** so it names `hello` and the goal.
5. Run **`create-handoff`** in Kiro → confirm **`docs/ai/active/handoff.md`** is filled.
6. Open the same folder in **Cursor**; prompt: *Implement `docs/ai/active/handoff.md` only; follow `docs/ai/03-standards.md`.*
7. Run whatever test command your stack uses (see `03-standards.md` or template defaults), e.g. **`npm test`** / **`pytest`** / **`go test ./...`**.

Worked files to compare: [examples/simple-feature](https://github.com/LaProgrammerie/ai-engineering-template/tree/main/examples/simple-feature).

---

## When to update which layer

**Rule of thumb**

- Change **global** when a habit or workflow should apply to **all** your repos (e.g. how you want reviews formatted, or a new global skill).
- Change **project** when something is **specific to this product** (stack, boundaries, active feature spec, handoff for today’s session).

The template maintains the authoritative **“if you change X, update Y”** table: [What to update when](https://github.com/LaProgrammerie/ai-engineering-template#4-what-to-update-when).

### Cheat sheet (which file to touch)

| You changed… | Update |
|--------------|--------|
| **Product** / durable scope | `docs/ai/01-product.md` |
| **Architecture** / boundaries | `docs/ai/02-architecture.md` (+ `docs/ai/05-decisions.md` if it is a durable ADR) |
| **Feature** (requirements / design / tasks) | `.kiro/specs/<feature>/` **then** `docs/ai/active/current-spec.md` **and** `handoff.md` if execution scope changed |
| **Only the next task** (same spec, new session) | `docs/ai/active/handoff.md` |

---

## Anti-drift guarantees (what the framework enforces)

Not magic — **conventions + explicit files**:

- **Single source of truth per concern** in `docs/ai/` (product, architecture, standards, workflows, decisions).
- **Spec → projection → handoff** chain so intent is not only inside Kiro.
- **Global skills** (`context-sync`, `debugging`, `code-review`) that are trained to flag **spec vs code** and **stale handoff**.
- **Repo wins over global** where they conflict, so the product layer stays sovereign.

Full map and rules: [context-map.md](https://github.com/LaProgrammerie/ai-engineering-template/blob/main/docs/ai/context-map.md).

---

## Limits and expectations

- **Not magic.** Files do not stay aligned by themselves unless you maintain them or add hooks. The framework is **conventions + explicit artefacts**, not auto-healing infrastructure.
- **Requires discipline.** Ignoring `handoff.md`, never updating `current-spec.md`, or skipping the spec chain → agents **will** drift. **Discipline is the product** (see [ai-engineering-core — Why use it](https://github.com/LaProgrammerie/ai-engineering-core#why-use-it)).
- **Not for every task.** For a one-off throwaway script, this stack is **overkill**; it pays off on **shared, evolving** codebases and teams that care about traceability from intent to code.
- **Progressive adoption is fine**; full value shows up when **core + template + spec → handoff → code** are used together.

---

## Failure modes

The framework **does not fail technically** — it fails when **artefacts lie**:

| Mode | What goes wrong |
|------|------------------|
| **Spec not updated** | Code or decisions move on; `.kiro/specs/` still describes the old intent. People and agents argue from different sources. |
| **Handoff outdated** | Spec or `current-spec.md` changed; `handoff.md` still names old files, tasks, or DoD. Implementation chases the wrong contract. |
| **Scope ignored** | “While you’re here” prompts go beyond `handoff.md`. Traceability and blameless review collapse. |
| **Mixing spec and implementation** | Requirements smuggle in implementation detail (or specs pretend the design is already coded). Hard to review, revert, or reuse. |
| **Over-reliance on AI** | No human reads handoff or diff; merges assumed safe; skills treated as proof. Errors scale with model confidence. |

### How to avoid

- **Sync spec → handoff** — After any material spec change, refresh **`create-handoff`** / **`handoff.md`** before the next coding session.
- **Treat `handoff.md` as the contract** — Not in the handoff → not in scope for this pass (or update the handoff first).
- **Update docs when scope changes** — Cascade **`current-spec.md`** and canon **`docs/ai/*`** per the [cheat sheet](#cheat-sheet-which-file-to-touch) and template [What to update when](https://github.com/LaProgrammerie/ai-engineering-template#4-what-to-update-when).

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

**Distribution:** commit **`docs/demo.gif`** after recording (see **[Demo](#demo)** and [`docs/demo-script.md`](docs/demo-script.md)).

---

## License

This umbrella repo is released under the [MIT License](LICENSE) (same spirit as the sibling repositories).

---

## GitHub repository metadata (suggested)

- **Description:** `Umbrella: AI engineering framework — global Kiro layer (ai-engineering-core) + project template (ai-engineering-template). Spec → handoff → code.`
- **Topics:** `ai`, `kiro`, `cursor`, `context-engineering`, `spec-driven-development`, `developer-tools`, `workflow`, `documentation`, `agents`
