---
title: Implement the Amendment Mechanism
date: 2026-09-09
status: ready-for-dev
type: feat
---

## Implement the Amendment Mechanism

Make it possible to change a finished task's intent, extend its plan, and re-run implementation — a new amend-task skill plus the scoping clauses the existing pipeline skills need, not either alone.

The spec is section 5 and the sections it cites, section 3 for the entry points and re-attach, section 4 for how a retraction is recorded: `git show task/002-amend-task-re-implement:docs/research/2026-09-09-amend-task-re-implement.md`. Follow it; don't re-derive it.

### Acceptance Criteria

- Every change the spec prescribes is in place, including each row of section 5's site/change table.
- Amend-task runs its own steps in this order: ensure the workspace, refuse if an unimplemented amendment is already there, write the criteria delta and the amendments entry once the human confirms, bump the status, then get the plan delta from plan-task, then re-gate through clarify. The spec doesn't pin this and the order matters — plan-task's delta-only mode keys on the new status, so the bump must precede it, and clarify's skip keys on the plan delta existing, so the re-gate must follow it.
- Section 5's fix to auto-task Step 0.3 extends to every task read that happens before the worktree exists — Steps 0.2 and 0.5, and the reads clarify itself performs when Step 0.3 hands it the task. Otherwise a mid-amendment run reports a stale status, picks its mode from stale criteria, and pressure-tests the pre-amendment task.
- Amending task 003 itself works end to end: take it to ready-for-signoff, amend it so that at least one existing criterion is retracted or replaced, re-run, and ship 003 with that amendment included.
- The README gains a short subsection under Tasks covering amend, re-run, ship.

### Notes

- The demonstration only counts if the amendment runs through the branch's own skill text, not an agent's reading of it. That means pointing the plugin marketplace at the task worktree and reloading plugins for the run, then pointing it back.
- Pin the per-amendment notes heading to a literal that matches its plan block, so the one writer and the five readers of the signal key on the same string.
- Impl-task's resume rule gets one clause naming the signal — the first amendment step with no landed commit — not a matching sub-procedure. The spec settles that a resume matches steps against commits and explicitly leaves the prescriptiveness here.
- Amend-task owns the authoring rules for the amendments log. Create-task gains one line naming the section and placing it after Implementation Notes, since it is the only place a reader learns what sections a task file has.
- Don't ship this before task 002 is on main — until then the spec exists only on that branch.
- Amending an epic sub-task stays open. File a follow-up task; don't solve it here.
- Version bump, release and the demo GIF are out of scope.
