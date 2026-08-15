---
name: update-feature-journey
description: Use when the user wants to review/sync the feature list and user journey documents against the current backlog and specs for the my-coffee-store project, or wants to generate/refresh `docs/01-requirements/02-plan/feature-list.md` and `docs/02-design/01-prototypes/user-journey.md` from `docs/01-requirements/backlog.md`. Trigger phrases include "สร้าง feature list", "อัปเดต feature list", "sync feature list กับ backlog", "ตรวจสอบ feature list", "สร้าง/อัปเดต user journey", "เช็คว่า feature list กับ backlog ตรงกันไหม". Also use after new spec documents are added via the create-requirement skill, to keep the feature list and user journey current.
---

# Update Feature List & User Journey

Check the requirements backlog/specs for drift against the existing feature
list and user journey documents, then regenerate or refresh both — following
the documentation pipeline defined in the project's `CLAUDE.md`.

This skill runs the checking and any user-facing clarification in the main
conversation, then delegates the actual file-writing to two subagents in
sequence (Agent/Task tool):
`feature-list-writer` first, then `user-journey-writer` — in that order,
because journey diagrams map back to features in the feature list, so the
feature list must be current before journeys are drawn from it.

## Steps

### 1. Check consistency (do this yourself, don't delegate)

Read:
- `docs/01-requirements/backlog.md`
- every non-archived spec under `docs/01-requirements/01-spec/*.md`
- `docs/01-requirements/02-plan/feature-list.md` (if it exists)
- `docs/02-design/01-prototypes/user-journey.md` (if it exists)

Compare and note any drift:
- Backlog rows (status ≠ `Archived`) with no matching row in
  `feature-list.md`.
- `feature-list.md` rows that point at a spec which is now archived/removed.
- Must/Should-have features in `feature-list.md` with no persona journey
  covering them in `user-journey.md`.

If you find drift, mention it briefly to the user in Thai before proceeding
(transparency, not permission-seeking). If nothing exists yet (first run),
just say so and proceed straight to generation.

### 2. Clarify ambiguity if needed

If something is genuinely unclear before you can delegate — a spec that
doesn't clearly support a Must vs. Should call, or a flow whose branch/step
isn't specified anywhere — ask the user directly (`AskUserQuestion` if
available, otherwise plain text), with **at least 3 concrete options and
their trade-offs**. Don't over-ask: only stop for calls that would
meaningfully change a durable document. If the subagents hit their own
ambiguity mid-run, they'll ask via their own `AskUserQuestion` calls (or
surface the question back to you to relay) — pass those along the same way.

### 3. Delegate to `feature-list-writer`

Invoke with `subagent_type: feature-list-writer`, passing:
- The drift findings from step 1 relevant to the feature list.
- Any clarifications from step 2.

Let it create/update `docs/01-requirements/02-plan/feature-list.md` (table +
MoSCoW + per-feature description).

### 4. Delegate to `user-journey-writer`

Only after step 3 completes. Invoke with `subagent_type:
user-journey-writer`, passing:
- The drift findings from step 1 relevant to journeys.
- What changed in the feature list from step 3 (new/changed/removed
  features and their MoSCoW bucket), so journey scope stays in sync.

Let it create/update `docs/02-design/01-prototypes/user-journey.md`
(Mermaid diagram + requirement-mapping table per persona journey).

### 5. Update cross-links

Check `docs/01-requirements/02-plan/index.md` and
`docs/02-design/01-prototypes/index.md` — if they don't already wikilink to
the new/updated documents, add a link following the existing
`[[relative/path/index|display text]]` pattern used throughout the vault.

### 6. Log the work

Append a dated entry to `docs/05-log/{YYYYMMDD}-log.md` (today's date). If
that file already exists, append a new section rather than overwriting; if
it doesn't exist, create it following the style of `docs/05-log/index.md`
and the existing `docs/05-log/20260802-log.md`. Include: drift found in step
1, what `feature-list-writer` changed, what `user-journey-writer` changed,
and any clarifying questions asked/answered.

### 7. Report back to the user

Summarize in Thai, concisely:
- Drift found in step 1 and how it was resolved.
- `feature-list.md` path, with a quick MoSCoW breakdown (counts per bucket).
- `user-journey.md` path, with which persona journeys were added/changed.
- Clickable links to every file touched.
- Any open question still unresolved.

## Notes

- This project has no source code, build, lint, or test commands — never
  invent or suggest any.
- Never delete a feature row or journey section; superseded/deprioritized
  ones get a status note instead, archived docs move to `docs/00-archived/`
  per `CLAUDE.md`.
- If `docs/01-requirements/02-plan/feature-list.md` or
  `docs/02-design/01-prototypes/user-journey.md` don't exist yet, they get
  created fresh as part of this flow — don't ask permission to create them,
  it's the normal first run of this pipeline stage.
- Run `feature-list-writer` and `user-journey-writer` **sequentially, not in
  parallel** — the second depends on the first's output.
