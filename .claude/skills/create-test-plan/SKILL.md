---
name: create-test-plan
description: Use when the user wants to create or update a test plan/test case document for a feature in the my-coffee-store project, or asks about acceptance-criteria test coverage. Trigger phrases include "ทำ test plan", "สร้าง test case ให้หน่อย", "อัปเดต test plan", "เช็ค acceptance criteria ครอบคลุมหรือยัง", "ขอ test plan ของฟีเจอร์ X". The user can scope it to specific requirement(s)/feature(s), or leave it default (all Must/Should-have features in feature-list.md without a test plan yet). Always proposes a plan (target file, new-vs-update decision, planned test cases) for the user to review and confirm before writing anything.
---

# Create Test Plan

Turn a spec's acceptance criteria/business rules plus the matching user
journey into a traceable test plan (objective, scope, test data, test case
table, acceptance-criteria coverage) for the my-coffee-store Obsidian vault —
following the documentation pipeline defined in the project's `CLAUDE.md`.

This skill runs the interactive part (scope confirmation, the
new-file-vs-update decision, and the final plan sign-off) in the main
conversation, then delegates the actual file-writing to the
`test-plan-writer` subagent (Agent/Task tool, `subagent_type:
test-plan-writer`), which has the detailed template for the test plan
document.

**Never skip straight to writing files.** Every run — first time or repeat —
ends with an explicit plan shown to the user and a yes/confirm before you
invoke `test-plan-writer`.

## Steps

### 1. Determine scope

- If the user named specific requirement(s)/feature(s)/spec(s) in their
  request, use exactly those.
- Otherwise, default to every **Must have** and **Should have** feature in
  `docs/01-requirements/02-plan/feature-list.md` that doesn't already have a
  test plan file under `docs/03-testing/01-test-plan/` (match by the spec's
  running number, e.g. feature spec `...-001-...` ↔ test plan `...-001-...`).
  State this default explicitly in the plan (step 4) so the user can narrow
  or expand it before confirming.
- Read the full spec document(s) under `docs/01-requirements/01-spec/` for
  every feature in scope — the Acceptance Criteria and Business Rules
  sections are the actual source material test cases will be derived from.
- Read `docs/02-design/01-prototypes/user-journey.md` for the persona
  flow(s) each feature belongs to. If a feature in scope has no journey yet,
  mention this to the user — the `update-feature-journey` skill may need to
  run first.
- Glob `docs/02-design/01-prototypes/prototypes/v*/` for the latest version
  folder — if a prototype screen exists for a feature in scope, note it so
  the test plan can reference the concrete wireframe. Not having one yet is
  fine; don't block on it.

### 2. Decide new-test-plan-file vs. update-existing-file (per feature)

Glob `docs/03-testing/01-test-plan/*.md` (excluding `index.md`) and match
filenames against the running number of each spec in scope.

- **No test plan file exists yet for this feature**: no need to ask — create
  a new file, per this vault's "first coverage of a pipeline stage needs no
  permission" convention. Say so in the plan (step 4).
- **A test plan file already exists for this feature**: decide what kind of
  change this run is, and recommend accordingly (but let the user confirm):
  - Recommend **เพิ่ม/แก้ test case ในไฟล์เดิม** when: the spec hasn't
    fundamentally changed, or you're only adding coverage for a business
    rule/acceptance criterion that was missing. This is the common case —
    test plans are living documents per feature, not versioned snapshots
    like prototypes, so there's no folder-version question to ask each time.
  - Recommend **เขียนใหม่ทั้งไฟล์ (regenerate)** only when the underlying
    spec changed enough that most existing test cases are stale (e.g. the
    flow was significantly redesigned) — flag this explicitly since it
    discards existing `สถานะ` (pass/fail) values already recorded on old
    test cases, which is a real loss the user should approve knowingly.
  State the recommendation and reasoning for *this specific request*, but
  let the user make the final call if it's not obviously the common case.

### 3. Identify the test cases to add/change

Before proposing the plan, sketch (don't write to disk yet) which test cases
this run will produce, each traceable to a specific acceptance criterion or
business rule sentence in the spec(s), plus the journey step it exercises.
Also note any acceptance criterion that has no clear way to test yet (a spec
gap) — these become `Blocked` test cases, not invented behavior.

### 4. Propose the plan and get confirmation — always, before writing anything

Present to the user, in Thai, before invoking `test-plan-writer`:

- **ขอบเขต**: which feature(s)/spec(s) (with MoSCoW bucket).
- **ปลายทาง**: `docs/03-testing/01-test-plan/{file}.md` — new file or
  updating an existing one in place, per the step-2 decision.
- **Test case ที่จะเพิ่ม/แก้**: list of test cases planned, each with a
  one-line description and which acceptance criterion/business rule it
  covers.
- **ช่องว่างที่พบ**: any acceptance criterion with no clear expected result
  yet (will be written as `Blocked`, not guessed).

Ask for explicit confirmation. If the user wants changes (different scope,
different new-vs-update decision, different test cases), adjust and
re-propose — don't proceed on an implicit "looks fine" silence.

### 5. Delegate to `test-plan-writer`

Invoke with `subagent_type: test-plan-writer`, passing:
- The confirmed scope (feature/spec(s)).
- The target test plan file path and new-vs-update decision from step 2.
- The confirmed test case list from step 3/4.
- Any prototype screen paths noted in step 1 worth cross-referencing.

Let it write/update the test plan document, including the mandatory
Acceptance Criteria Coverage table.

### 6. Update cross-links

- `docs/03-testing/01-test-plan/index.md` — make sure every test plan file
  is linked. Once a second test plan file exists, convert the index's
  content from a single inline bullet reference into a table (No. / ฟีเจอร์ /
  สเปค / ลิงก์), matching the style already used in e.g.
  `docs/02-design/01-prototypes/prototypes/index.md` — don't leave it as an
  informal bullet list once there's more than one entry to track.

### 7. Log the work

Append a dated entry to `docs/05-log/{YYYYMMDD}-log.md` (today's date). If it
already exists, append a new section; if not, create it following
`docs/05-log/index.md`'s style. Include: scope decided in step 1, the
new-vs-update decision from step 2 and why, every test case added/changed,
and any acceptance criterion left uncovered.

### 8. Report back to the user

Summarize in Thai, concisely:
- Scope covered (features).
- Test plan file used, new-vs-update and the reasoning.
- Every test case added/changed, with clickable links to the file.
- Acceptance Criteria Coverage result — flag anything still uncovered.
- Any open question or gap flagged (e.g. a spec that doesn't define expected
  behavior a test case needed).

## Notes

- This project has no source code, build, lint, or test commands — a "test
  plan" here is a Markdown QA document (objective/scope/test cases), never
  an executable test suite or CI config. Don't suggest otherwise.
- Never delete a test plan file or a test case row — old/obsolete coverage
  gets a status note instead; move a fully obsolete test plan to
  `docs/00-archived/` per `CLAUDE.md`, don't delete it.
- Actual test **results** (pass/fail, bugs found) don't belong in this
  skill's output — they go in `docs/03-testing/02-test-result/`, a separate
  concern with its own (not-yet-built) tooling. If the user asks to record
  results rather than plan tests, say so rather than writing results into a
  test plan document.
- If `test-plan-writer` comes back with its own clarifying question (spec
  coverage ambiguity, an acceptance criterion testable multiple ways), relay
  it to the user yourself with the same ≥3-option format, then re-invoke the
  subagent with the answer.
