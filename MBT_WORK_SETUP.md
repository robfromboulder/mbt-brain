# Mini-Brain Toolkit: Work Setup

> V1, 2026-11-11.

Procedure for scaffolding a new work item's working docs from an intake conversation. The bookend to `MBT_WORK_CLOSEOUT.md`: setup creates the `working/` docs when a work item opens, closeout folds them into the canonical mini-brain when the work concludes. Read `CLAUDE.md` first for file conventions — they govern every file this procedure touches.

Run this when starting a new work item — **code-changing work that gets its own branch and PR**: a feature, a bug fix, or a hardening effort — or when formalizing a partial one that already lives in `working/` (e.g. a PLAN+FINDINGS pair that needs the rest of its scaffolding). The output is the `working/MBT_<WORK>_*.md` documents that track a work item until it concludes. `MBT` throughout is the owning lobe's lobespace — in a brain with one lobe, the brain's own lobespace. Setup is **additive and idempotent**: it creates only the docs that are missing and never overwrites existing work, so it is safe to re-run as a work item grows.

The scope is deliberately narrow and practical: **assume a work item has a branch and a PR until proven otherwise.** It is often not knowable at inception whether an idea is a shallow doc/config tweak or something deeper, so default to the full scaffold rather than guessing small — an oversized scaffold is cheap, and under-scaffolding something that turns out deep is not. If the work later concludes without a code change, closeout still folds in whatever knowledge the docs hold (see `MBT_WORK_CLOSEOUT.md`); the burndown does not apply. A pure question that produces no artifact worth keeping needs no work item at all: answer it, and if the answer is worth recording, add a session-log entry.

The working docs and their roles:

| Doc | Role | Seeded at setup from the chat? |
|---|---|---|
| `MBT_<WORK>_PLAN.md` | The spec: objective, what changes, testing approach, scope boundary, open issues | Yes — written from the intake conversation |
| `MBT_<WORK>_FINDINGS.md` | The decision record: problem, preferred approach, tradeoffs, alternatives | Yes — written from the intake conversation |
| `MBT_<WORK>_LOG.md` | Append-only session log; one entry per implementation session | No — header only; the first session appends the first entry |
| `MBT_<WORK>_BURNDOWN.md` | Finishing checklist: what remains to land the change | No — template checklist |
| `MBT_<WORK>_CLAUDE.md` | Runbook for automated tests — declares the item's branch (the session-routing join key), defers to the brain's canonical runbook, then adds work-specific steps | Test names only — one step per automated test in the testing approach; commands and expected results stay placeholders |
| `MBT_<WORK>_TESTING.md` | Manual test plan: steps to verify the work item by hand; folds into the canonical testing doc at closeout | Case names only — one case per manual check in the testing approach; prerequisites, steps and expected results stay placeholders |

PLAN and BURNDOWN are two halves of the same work: the plan gets the code written, the burndown gets it merged. The burndown's Routine rows are inherited defaults: a brain adopting this procedure refines them once, outside any setup run, to match its PR conventions — the closeout-notes row is not a PR convention and stays. Custom rows are always per-item; the scaffold's category placeholders there are prompts to fill or strike, not standing work.

These are **working files**, so per `CLAUDE.md` they are *not* versioned (no `> V<N>` header), are *not* added to the read index, and are not read in future sessions unless explicitly asked. This procedure's own alignment step (§2) is the one exception: it reads other open items' PLAN and FINDINGS.

---

## 1. The intake conversation

Before scaffolding anything, have the conversation. Setup turns a settled discussion into documents; it does not invent the design. Pin down, at minimum:

- **Owning lobe** — in a multi-lobe brain, which lobe the work belongs to: the nearest common ancestor of the lobes the work will concern. That lobe's `working/` holds the docs and its lobespace becomes their `MBT`. Settle this first, since it decides both. A brain with one lobe skips this and uses the brain's own lobespace.
- **Work-item name** → derive the `<WORK>` slug (§2).
- **One-line definition** — what the work item is, in a sentence.
- **Problem** — what's broken or missing, and who feels it. This anchors FINDINGS.
- **Preferred approach + tradeoffs** — the chosen direction and *why*, plus the alternatives weighed and rejected. This is the heart of FINDINGS; capture the reasoning, not just the verdict.
- **Scope boundary** — what the work item explicitly does *not* touch. Goes into PLAN's "what this does NOT include"; it is the cheapest way to prevent scope creep.
- **Testing approach** — what's covered by automated tests and what can only be checked by hand. Seeds PLAN's testing section and the test names in the runbook and testing docs.

If the chosen approach makes an existing canonical claim false (a reversal, not just an addition), note it now — record it in FINDINGS and flag it for closeout. A reversal is the one thing a wording-based grep won't catch at merge time, so the merge relies on it being called out explicitly here.

---

## 2. Derive the slug, audit what exists, and align

**Slug.** Every filename this procedure produces is `MBT_<WORK>_*` — derive `<WORK>` from the work-item name and lead with the owning lobe's lobespace, always. `<WORK>` is a SCREAMING_SNAKE_CASE slug and never carries a lobe's lobespace, its own or another's. A handed SCREAMING_SNAKE identifier is the slug verbatim, minus any lobe's lobespace it opens with, taking the longest match (a handed `MBT_FOO` gives `<WORK>` = `FOO` and files `MBT_FOO_*`); any other handed name gets a short, specific slug written for it. Check the compound `MBT_<WORK>` before use, rewording the slug — handed or written — until both checks pass. Distinctness in the **prefix** sense: the procedures find an item's docs by the glob `MBT_<WORK>_*`, so `MBT_<WORK>_` must not begin any canonical document's name or the compound of another open item in `working/`, and no other item's compound may begin this one's — a slug `PLUM` beside an item slugged `PLUM_JAM` would sweep that item's docs into its own closeout. And in a multi-lobe brain: no other lobe's lobespace may be a longer prefix of `MBT_<WORK>` than `MBT` itself — ownership goes to the longest match, so a parent's slug that continues into a child's lobespace hands the item's files to that child. Names in `archive/` don't count. The fuller name lives in the `# <Work-Item Name>` heading.

**Audit (this is what makes setup safe on partial/existing work items).** Before creating anything, list the owning lobe's `working/` for the slug. From the owning lobe's directory — the repo root for a single-lobe brain:

```bash
find working -maxdepth 1 -name 'MBT_<WORK>_*.md' 2>/dev/null
```

The audit does not search `archive/`. Its stale, abandoned and older-convention material can carry names that match a new slug without belonging to it. Empty output means a brand-new item **only if the command ran in the owning lobe's directory** — confirm the location before trusting an empty audit, because everything below builds on it.

Classify each doc — first match wins:
- **Exists in `working/`** — leave it. Do not overwrite. If the intake chat adds genuinely new material, add it under its matching section and leave every existing line unchanged.
- **Missing PLAN on an item with other docs in `working/`** — ask the developer whether the PLAN was retired early before creating one; if it was, don't recreate it.
- **Missing** — create from the template (§3).

A brand-new work item has nothing on disk, so the full set is created. A partial one (commonly PLAN+FINDINGS) gets only its missing docs created.

**Align against other open items.** Read the PLAN and FINDINGS of every other open work item in the `working/` of each lobe the work concerns, the owning lobe, and each of the owning lobe's ancestors — in a brain with one lobe, the owning lobe alone. From each such lobe's directory, with `<L>` that lobe's lobespace:

```bash
find working -maxdepth 1 \( -name '<L>_*_PLAN.md' -o -name '<L>_*_FINDINGS.md' \) 2>/dev/null
```

Group the matches by compound, the name before `_PLAN.md` or `_FINDINGS.md`, and skip this item's own. Record each overlap or conflict in this item's PLAN under open issues — written into the PLAN §3 creates, or added to an existing PLAN without changing its existing lines. When the PLAN was retired early, record it in FINDINGS under an `## Open issues` heading instead, adding the heading if FINDINGS lacks one.

---

## 3. Generate the docs

Create each missing doc in the owning lobe's `working/`, from its base template, substituting `MBT` (the owning lobe's lobespace), `<WORK>` (SCREAMING_SNAKE), `<Work-Item Name>` (Title Case), and `<work-branch>` — a single branch name, or a glob when the item will land across several branches/PRs; setup declares the name — creating the branch belongs to the project's workflow, when coding starts. Create that `working/` if the lobe does not have one yet. Seed each doc as far as the table's seeded column says; everything else stays a scaffold for the implementer.

Do not add a `> V<N>` version header to any of these — working files are unversioned. Do not copy them into `archive/` (docs land there only when they retire). Do not touch the read index or any canonical doc.

The base template for each doc is inlined below, so this brain stands alone without the toolkit.

### `MBT_<WORK>_PLAN.md`

````markdown
# <Work-Item Name>: Implementation Plan

## Objective

<one paragraph from the intake chat: what the work item does and the high-level approach. Close with the "no new X" constraints if any — e.g. "No new infrastructure, no database changes.">

## Context

See `MBT_<WORK>_FINDINGS.md` in this directory for the decision record and alternatives considered.

Key references in the owning lobe:
- `MBT_SCOPE.md` — problem space this fits into
- `MBT_FINDINGS.md` — relevant prior decisions
- `MBT_APPROACH.md` — design constraints

## What changes

<the concrete edits, file by file. Fill in during/just before implementation; leave a stub here at setup if the details aren't settled yet.>

## Interaction with existing code

| Existing code | Interaction | Risk |
|---|---|---|
|  |  |  |

## Testing approach

<automated: unit vs. integration, new test classes anticipated, what regression suite must stay green. Manual steps live in `MBT_<WORK>_TESTING.md`.>

## Implementation sequence

1.

## Scope boundary — what this does NOT include

- <the explicit non-goals settled in the chat>

## Open issues

1. <unresolved tradeoffs or decisions deferred to implementation>
````

### `MBT_<WORK>_FINDINGS.md`

````markdown
# <Work-Item Name>: Decision Record

## Problem

<the problem from the chat: what's broken/missing, who feels it, why the status quo is inadequate.>

## Preferred approach: <short name>

<the chosen direction and how it works.>

| Aspect | Detail |
|---|---|
|  |  |

- **<property>** — <why this approach wins on it>

## Alternatives considered

- **<alternative>** — <what it was and why it was rejected.>

## What this doesn't solve

- <limitations that remain even within the chosen approach — every ground-truth FINDINGS doc carries this section.>

## Reversals

<If this work item makes an existing canonical claim false, name the exact claim and file here. `MBT_WORK_CLOSEOUT.md` will reconcile it in place at merge. Delete this section if nothing is reversed.>
````

### `MBT_<WORK>_LOG.md`

````markdown
# <Work-Item Name>: Session Log

Running log of implementation sessions. Each session appends after a `---` separator.

The first entry, and every one after it, follows the entry format and content rules in `MBT_SESSION_CLOSEOUT.md` — that spec is the authority, not the prior entry, since a fresh LOG file has none to copy.
````

### `MBT_<WORK>_BURNDOWN.md`

````markdown
# <Work-Item Name>: Burndown

Everything between working code and a merged PR.

## Routine

* commit and push the finished change on the work branch
* create draft PR
* review code changes
* full rebuild and automated retest on PR code
* review PR description against final code one last time
* request feedback from reviewers
* respond to review feedback
* automated checks — see `MBT_<WORK>_CLAUDE.md`
* manual checks — capture steps in `MBT_<WORK>_TESTING.md`
* closeout notes — <deviations from the standard flow, and any canonical claim this work item reverses — see `MBT_WORK_CLOSEOUT.md`>

## Custom

* docs — <screenshots, user-facing docs, if any>
* porting/backporting — <if this work ships to more than one target>
* <follow-on items discovered during implementation>
````

### `MBT_<WORK>_CLAUDE.md`

````markdown
# <Work-Item Name>: Claude Runbook

Instructions for branch: `<work-branch>`

## Running Tests

Run the steps from the brain's canonical runbook first (if it has one), verifying results match the expectations documented there. Then run the work-specific step(s) below.

### Step N — <work-specific test> ← NEW in this branch

```bash
# fill in once the work item has tests
```

**Expected:** <runtime, pass/fail expectations, which tests are active>
````

### `MBT_<WORK>_TESTING.md`

````markdown
# <Work-Item Name>: Manual Test Plan

Checks run by hand for this work item (UI, API calls, data checks). At closeout these fold into the canonical testing doc.

## Setup

<prerequisites: config flags, roles/users, sample data needed before the steps below>

## Test cases

1. **<case name>** — the steps to exercise it by hand.
   - **Expected:** <observable result>
````

---

## 4. Handoff

After creating the docs:

1. Report the owning lobe, the slug used, which docs were **created**, which already **existed** in `working/`, and any doc not recreated because it was retired early.
2. Remind that these are working files: they stay out of the read index, and the canonical mini-brain is untouched until the work concludes.
3. Point at `MBT_WORK_CLOSEOUT.md` as the closeout bookend — it consumes exactly these docs.
