---
name: create-prototype
description: Use when the user wants to generate or update a UI prototype/wireframe for the my-coffee-store project, drawing on requirements, backlog, feature list, and user journey together. Trigger phrases include "สร้าง prototype", "ทำ mockup ให้หน่อย", "อัปเดต prototype", "ขอปรับ prototype เดิม", "สร้าง wireframe ของฟีเจอร์ X". The user can scope it to specific requirement(s)/feature(s), or leave it default (all Must/Should-have features not yet prototyped). Always proposes a plan for the user to review and confirm before writing anything, always asks (with a recommendation) whether repeat runs should create a new version folder or edit the latest one, and creates `docs/02-design/01-prototypes/DESIGN.md` first (with user input) if it doesn't exist yet.
---

# Create Prototype

Turn requirements + backlog + feature list + user journey into text-based
(Markdown wireframe) UI prototypes for the my-coffee-store Obsidian vault —
following the documentation pipeline defined in the project's `CLAUDE.md`.

This skill runs every interactive part (scope confirmation, design-system
style intake, the version-folder decision, and the final plan sign-off) in
the main conversation, then delegates the actual file-writing to the
`prototype-writer` subagent (Agent/Task tool, `subagent_type:
prototype-writer`), which has the detailed templates for `DESIGN.md` and for
each screen file.

**Never skip straight to writing files.** Every run — first time or repeat —
ends with an explicit plan shown to the user and a yes/confirm before you
invoke `prototype-writer`.

## Steps

### 1. Determine scope

- If the user named specific requirement(s)/feature(s)/spec(s) in their
  request, use exactly those.
- If the user explicitly asks for an **interactive**/clickable **HTML**
  prototype (not just "prototype" or "wireframe"), note that this run
  additionally needs Workflow C — see step 5a. This is an optional
  supplement per `CLAUDE.md`'s "ข้อยกเว้น: Interactive Prototype" exception;
  Markdown wireframes are still required as the base output for every
  screen in scope, regardless of whether HTML is also requested.
- Otherwise, default to every **Must have** and **Should have** feature in
  `docs/01-requirements/02-plan/feature-list.md` that doesn't already have a
  prototype screen in the latest version folder (see step 4). State this
  default explicitly in the plan (step 6) so the user can narrow or expand it
  before confirming — don't silently assume it's what they want.
- Read `docs/01-requirements/01-spec/*.md` for every feature in scope, and
  `docs/02-design/01-prototypes/user-journey.md` for the persona flow(s) each
  feature belongs to. If a feature in scope has no journey yet, mention this
  to the user — the `update-feature-journey` skill may need to run first.

### 2. Ensure `DESIGN.md` exists

Check `docs/02-design/01-prototypes/DESIGN.md`.

- **If it exists**: read it fully. It's the single source of truth for every
  color/font/component the prototype screens will reference. Don't ask the
  user about style again.
- **If it doesn't exist**: tell the user, then ask (`AskUserQuestion`) how
  they want to define it — present exactly these three paths:
  1. **เลือกโทนสี/สไตล์จาก preset** — e.g. "earth tone + minimalist + muji" (the
     tone already used in this vault previously) or another named style the
     user picks.
  2. **อธิบายโทนสี/สไตล์เอง** — free text description of colors/mood/references.
  3. **ส่งภาพโลโก้หรือภาพตัวอย่าง** — the user attaches/points to an image; ask
     them to save it under `docs/02-design/01-prototypes/assets/` and give you
     the path (you can't read attachments directly off disk — see the
     earlier logo-handling precedent in this project's log), then inspect it
     yourself with `Read` before extracting colors/style.
  Once you have style input, delegate **Workflow A** to `prototype-writer`
  to draft `DESIGN.md`, passing along the style input verbatim. Show the
  user the resulting file and get their OK before moving to step 3 — a brand
  identity document is durable and shouldn't be created without a look.

### 3. Decide new-version-folder vs. edit-latest-folder

Glob `docs/02-design/01-prototypes/prototypes/v*/`.

- **No version folder exists yet (first run)**: no need to ask — create
  `v1` directly, per this vault's "first run of a pipeline stage needs no
  permission" convention. Say so in the plan (step 6), don't skip it silently.
- **At least one version folder exists (repeat run)**: **always ask**, every
  time, even if the change seems small. Use `AskUserQuestion` with a
  recommendation attached, computed from the nature of this run's change:
  - Recommend **สร้าง Folder Version ใหม่ (v{N+1})** when: the scope adds a
    new requirement/feature, the request is a significant redesign, or the
    user wants to compare before/after (e.g. for a stakeholder review).
    Trade-off: keeps full history, but more files to maintain over time.
  - Recommend **แก้ไข Folder ล่าสุด (v{N})** when: the request is a small
    fix/tweak to existing screens (typo, wrong copy, minor layout change) and
    no one has a reason to compare against the old version.
    Trade-off: no history of the "before" state, but avoids version sprawl
    for minor edits.
  State which one you'd recommend and why for *this specific request*, but
  let the user make the final call — don't decide silently either way.

### 4. Propose the plan and get confirmation — always, before writing anything

Present to the user, in Thai, before invoking `prototype-writer` for
Workflow B:

- **ขอบเขต**: which features/specs (with MoSCoW bucket), and which personas.
- **DESIGN.md**: existing (link it) or newly created in step 2 (link it).
- **ปลายทาง**: `docs/02-design/01-prototypes/prototypes/v{N}/`, new folder or
  editing v{N} in place — per the step-3 decision.
- **หน้าจอที่จะสร้าง/แก้ไข**: list of screen files planned, one per journey
  step/persona flow in scope, with a one-line description each.
- **Interactive HTML (ถ้าผู้ใช้ขอ)**: state explicitly that this is an
  optional supplement living at `prototypes/v{N}/interactive/`, alongside
  (not instead of) the Markdown screens, and which flow/states it will
  demonstrate.

Ask for explicit confirmation. If the user wants changes (different scope,
different version decision, different screens), adjust and re-propose —
don't proceed on an implicit "looks fine" silence.

### 5. Delegate to `prototype-writer` (Workflow B)

Invoke with `subagent_type: prototype-writer`, passing:
- The confirmed scope (features/specs/personas).
- The `DESIGN.md` path.
- The target version folder and new-vs-edit decision from step 3.
- The confirmed screen list from step 4.

Let it write/update the version folder's `index.md` and one file per screen.

### 5a. Delegate Workflow C (only if interactive HTML was requested)

After Workflow B completes (the Markdown screens must exist first — Workflow
C supplements them), invoke `prototype-writer` again for Workflow C, passing:
- Which screens/flow in the just-written version folder to make interactive.
- Confirmation that this is additive — Markdown screens are not replaced.

Let it write the self-contained HTML/CSS/JS file(s) to
`prototypes/v{N}/interactive/` plus that subfolder's own `index.md`.

### 6. Update cross-links

- `docs/02-design/01-prototypes/index.md` — link to
  `prototypes/index.md`/the latest version if not already linked.
- If `docs/02-design/01-prototypes/prototypes/index.md` (a top-level list of
  all versions) doesn't exist yet, create it with one row per version folder
  (version, date, one-line scope summary, link) — update it whenever a
  version is added.

### 7. Log the work

Append a dated entry to `docs/05-log/{YYYYMMDD}-log.md` (today's date). If it
already exists, append a new section; if not, create it following
`docs/05-log/index.md`'s style. Include: scope decided in step 1, whether
`DESIGN.md` was created/reused, the new-vs-edit decision from step 3 and why,
and every screen file touched.

### 8. Report back to the user

Summarize in Thai, concisely:
- Scope covered (features/personas).
- `DESIGN.md` status (existing/created, link).
- Version folder used, new-vs-edit and the reasoning.
- Every screen file created/updated, with clickable links.
- Any open question or gap flagged (e.g. a spec that doesn't define a
  behavior a screen needed).

## Notes

- This project has no source code, build, lint, or test commands. Every
  screen in scope still gets a plain Markdown wireframe (ASCII box layout)
  as the required, default output. An interactive HTML/CSS/JS prototype is
  allowed **only** as an optional supplement, only when the user explicitly
  asks for one, and only under `prototypes/v{N}/interactive/` as
  self-contained static files (no build step, no package manifest, no
  external dependency) — per `CLAUDE.md`'s "ข้อยกเว้น: Interactive
  Prototype" exception. Don't default to HTML, and don't build it in place
  of the Markdown screens.
- Never delete a version folder or a screen file — old versions are kept as
  history. If a version is ever truly obsolete, move it under
  `docs/00-archived/` per `CLAUDE.md`, don't delete it.
- The version-folder question in step 3 is asked **every repeat run, no
  exceptions** — even if the previous run's answer seems like it should
  obviously apply again.
- If `prototype-writer` comes back with its own clarifying question (layout
  ambiguity, a missing DESIGN.md component, a spec gap), relay it to the user
  yourself with the same ≥3-option format, then re-invoke the subagent with
  the answer.
