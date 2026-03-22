# Cross-repo examples

**Status:** this **umbrella** repository does **not** contain worked example code or filled spec trees.  
Everything below lives in **sibling repositories** ([ai-engineering-template](https://github.com/LaProgrammerie/ai-engineering-template), [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core)) — this folder is only an **index and pointers**.

There are **no** copy-paste feature folders inside **ai-engineering-framework** itself; clone the linked paths if you want files on disk.

---

## Demo flow (quick)

Checklist for a minimal recording or dry run (detail: [`../docs/demo-script.md`](../docs/demo-script.md)):

1. `git clone` [ai-engineering-template](https://github.com/LaProgrammerie/ai-engineering-template) (or your fork) → `cd` into it.
2. Sync [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core) to `~/.kiro` and reload Kiro (`./sync-to-home.sh`).
3. In Kiro: create `.kiro/specs/<feature>/` with `requirements.md`, `design.md`, `tasks.md`.
4. Update `docs/ai/active/current-spec.md` to match; run skill **`create-handoff`** → `docs/ai/active/handoff.md`.
5. Open the repo in **Cursor**; implement strictly from `handoff.md` + `docs/ai/03-standards.md`.
6. Run your project’s test command from repo root (`npm test`, `pytest`, `go test ./...`, etc.).
7. Save recording as **`docs/demo.gif`** in [ai-engineering-framework](https://github.com/LaProgrammerie/ai-engineering-framework) for the umbrella README.

---

| Example | Repository | What you get |
|---------|------------|----------------|
| **simple-feature** | [ai-engineering-template](https://github.com/LaProgrammerie/ai-engineering-template/tree/main/examples/simple-feature) | Filled spec slice, `current-spec`, handoff, and reference code you can diff against empty placeholders. |
| **flow-login** (narrative) | [ai-engineering-core](https://github.com/LaProgrammerie/ai-engineering-core/blob/main/examples/flow-login.md) | Story of how global skills and the project template chain together for one feature. |

Open the links on GitHub or clone the sibling repos to use these examples locally.
