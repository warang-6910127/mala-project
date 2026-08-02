---
name: create-requirement
description: Use when the user gives a raw, informal requirement, feature idea, or business rule for the my-coffee-store project and wants it turned into a formal requirement spec document under `docs/01-requirements/01-spec/`, added to `docs/01-requirements/backlog.md`, and logged in `docs/05-log/`. Trigger phrases include "เพิ่ม requirement", "อยากได้ฟีเจอร์...", "สร้าง requirement ให้หน่อย", "ช่วยเขียนสเปคให้หน่อย", or any raw feature/business request that hasn't yet been written as a spec document. Also use when asked to reconcile the backlog or log with specs that already exist on disk.
---

# Create Requirement → Backlog

Turn a raw requirement from the user into a formal spec document, keep
`docs/01-requirements/backlog.md` in sync, and log the work — following the
documentation pipeline defined in the project's `CLAUDE.md`.

This skill runs the interactive part (talking to the user) in the main
conversation, then delegates the file-writing part to the
`requirement-writer` subagent (Agent/Task tool, `subagent_type:
requirement-writer`), which has the detailed workflow for filenames,
dedup/supersede analysis, spec structure, backlog rows, and log entries.

## Steps

1. **Collect the raw requirement.** Take whatever the user gave, in Thai or
   mixed language, as-is. Don't rewrite or formalize it yet — that happens
   in the spec document itself.

2. **Clarify anything unclear — in this conversation, not silently.** Before
   writing anything, check whether you have enough to produce a useful spec:
   scope (in/out), who it's for, acceptance criteria, any business rules
   with concrete numbers (prices, limits, thresholds). If something is
   genuinely ambiguous:
   - Ask the user directly (`AskUserQuestion` if available, otherwise ask in
     plain text).
   - **Every clarifying question must offer at least 3 concrete approaches
     to choose from** — never an open-ended "what do you mean?". Frame the
     options as real alternatives with trade-offs, e.g. "(a) จำกัดจำนวนคูปองต่อผู้ใช้
     1 ใบ, (b) จำกัดต่อวัน, (c) ไม่จำกัดแต่หมดอายุใน 7 วัน".
   - Repeat for each unclear point, but don't over-ask — only stop for things
     that would meaningfully change the spec's content.

3. **Check for related existing requirements yourself first.** Skim
   `docs/01-requirements/backlog.md` and glance over filenames in
   `docs/01-requirements/01-spec/` for topics that sound related to what the
   user is asking for. Mention to the user if you spot something that looks
   related, so the subagent's dedup analysis isn't a surprise — but the
   actual new-vs-edit-vs-supersede decision is the subagent's job (it reads
   full file contents, not just names).

4. **Delegate the writing.** Once the requirement is clarified enough,
   invoke the `requirement-writer` subagent with the clarified requirement
   (including today's date, and any related-doc hints from step 3). Let it:
   - decide new file vs. edit-existing vs. supersede-and-archive,
   - write the spec document to `docs/01-requirements/01-spec/`,
   - update `docs/01-requirements/backlog.md`,
   - append an entry to `docs/05-log/{YYYYMMDD}-log.md`.

   If the subagent comes back with its own clarifying question (it may hit
   ambiguity you didn't — e.g. about how a new requirement relates to an old
   one), ask the user that question yourself (again, ≥3 options), then
   re-invoke the subagent with the answer.

5. **Report back to the user.** Summarize in Thai, concisely: which spec file
   was created/edited, the new-vs-existing decision and why, the backlog row
   added, and the log entry — with clickable links to each file.

## Notes

- This project has no source code, build, lint, or test commands — never
  invent or suggest any.
- Never delete a requirement document; superseded ones move to
  `docs/00-archived/` (see `CLAUDE.md`).
- If `docs/01-requirements/backlog.md` or `docs/05-log/{date}-log.md` don't
  exist yet, they get created fresh following the existing `index.md` style
  — don't ask the user for permission to create these, they're a normal part
  of this pipeline.
