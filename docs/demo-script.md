# Demo script (record a GIF or video)

Use this in a repo bootstrapped from **[ai-engineering-template](https://github.com/LaProgrammerie/ai-engineering-template)** with **[ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core)** synced to `~/.kiro`. Adjust paths if your feature folder name differs.

**Output:** export the recording as **`docs/demo.gif`** in the **ai-engineering-framework** repo (or record from a template clone and copy the file here).

---

## Before recording

1. Terminal: `cd` into your product repo.
2. Kiro: open the same repo; Cursor: open the same repo.
3. Clear sensitive UI (tokens, internal URLs) if needed.

---

## Steps (chronological)

### 1. Create the spec

In **Kiro**, create a feature folder, e.g. `.kiro/specs/demo-feature/`, with at least:

- `requirements.md` — one small, verifiable goal (e.g. “add a function that returns a greeting”).
- `design.md` — brief approach.
- `tasks.md` — 2–4 checkable tasks.

*Show the tree or file tabs briefly so viewers see the spec layout.*

### 2. Write / show tasks

Keep **tasks.md** explicit (e.g. “Implement `greet()`”, “Add unit test”).

### 3. Sync projection + generate handoff

In the repo:

```bash
# Edit summary for other tools (adjust content to match your spec)
${EDITOR:-vim} docs/ai/active/current-spec.md
```

In **Kiro**, run the repo skill **`create-handoff`** (or paste the handoff template and fill it) so **`docs/ai/active/handoff.md`** lists allowed files, plan, and DoD.

*Show `handoff.md` in the editor—this is the execution contract.*

### 4. Open Cursor

Open the **same repository** in Cursor. Optionally mention: agent reads **`AGENTS.md`**, **`handoff.md`**, **`docs/ai/03-standards.md`**.

### 5. Implement

In Cursor chat or composer, prompt from **handoff** only, e.g.:

```text
Implement exactly what docs/ai/active/handoff.md says. Do not change files outside the allowed list.
```

*Show a small diff or file creation matching the handoff.*

### 6. Run tests

From repo root (adapt to your stack):

```bash
# Examples — use what 03-standards.md defines for this project
npm test
# or
pytest
# or
go test ./...
```

*Show the test command succeeding.*

---

## Export tips

- **Length:** ~30–90s; crop to the spec → handoff → Cursor → test arc.
- **Size:** keep GIF under ~5–10MB if possible (fps / resolution) for GitHub README load.
- **Tools:** OBS / QuickTime → ffmpeg, ScreenToGif, or similar.

---

## One-liner recap

**Spec (Kiro) → `current-spec.md` + `handoff.md` → implement in Cursor → run tests.**
