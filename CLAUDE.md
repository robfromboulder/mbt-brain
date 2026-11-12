# Mini-Brain Toolkit: Entrypoint

> V36, 2026-11-11.

---

## Read Index

All paths are relative to this repo root (`CLAUDE.md`'s directory). Before reading, state which files in one line and read them the same turn (not a permission request). Read only what the question needs.

| Purpose | File |
|---|---|
| Problem definition, the world it exists in, goals | `MBT_SCOPE.md` |
| Technical approach, key design decisions | `MBT_APPROACH.md` |
| Key implementation findings (invisible from the files) | `MBT_FINDINGS.md` |
| Enforced terms — the brain's reserved vocabulary | `MBT_ENFORCED_TERMS.md` |
| Research agenda — where the pattern could go, and how it compares to related work | `MBT_RESEARCH.md` |
| Comparable systems — the second-brain / agent-memory landscape (read during the dream cycle's competitive-landscape pass) | `MBT_COMPARABLES.md` |
| Cognitive-science grounding — biological models the pattern draws on (read during the dream cycle's biological-grounding pass) | `MBT_BIOLOGY.md` |
| Example brains outside the toolkit — provenance and shape, for testing against | `MBT_EXAMPLES.md` |
| Acceptance testing — the scenarios to run before a release, and how to run them | `MBT_TESTING.md` (read when asked to run the acceptance tests) |
| Session log — why we chose what we chose | `MBT_LOG.md` (read from last `---`; large) |
| Session log update format and rules | `MBT_SESSION_CLOSEOUT.md` (read when asked to update the session log) |
| Work setup — scaffold a work item's working docs | `MBT_WORK_SETUP.md` |
| Work closeout — fold working docs into the mini-brain | `MBT_WORK_CLOSEOUT.md` |
| Dream cycle — periodic self-improvement pass | `MBT_DREAM_CYCLE.md` (read when asked to dream) |
| Dream cycle session log | `MBT_DREAM_LOG.md` |

Only files in this table are current. `archive/` holds source material, retired/absorbed docs, and retired work-item working files — ignore it unless asked. `tests/` holds the scripts `MBT_TESTING.md` runs; they are current, and no procedure writes to them.

**The product.** This brain's project is the toolkit, the `mini-brain-toolkit` clone beside this repo. `SKILL.md`, `agents/`, `MBT_PATTERN.md`, `MBT_LOBES.md`, `MBT_CREATE_BRAIN.md`, `MBT_CHECK_BRAIN.md` and `templates/` name files there; every other path names a file in this repo. The product's code is its docs and templates, so a work item that changes only them is code-changing work.

`working/` holds experiments and a work item's in-flight `MBT_<WORK>_*` docs, where `<WORK>` is the item's slug. When a work item concludes, fold them into the canonical docs and **move** (not delete) them to `archive/`. A move never overwrites. `git mv` enforces this for tracked files; for an untracked file, check the destination before moving. If `archive/` already holds a file of an incoming file's name, stop and ask the developer for a new slug. Before moving any file, rename all of the item's files still in `working/` to that slug and rewrite their references to each other. The item's PLAN, once consumed (converted to code), may move there early at the developer's request — walk its significant unaddressed items with the developer first.

`MBT_TESTING.md` is the canonical testing doc: at closeout an item's `_TESTING` doc folds into it, as scenarios, the cases worth repeating at each release. This brain has no canonical runbook, so an item's `_CLAUDE` doc retires to `archive/` unfolded.

---

## File Conventions

**Top-level vs archive.** A top-level file is the canonical current version; its history lives in version control. `archive/` is a stash for source material and retired/absorbed docs — not in-tree version snapshots.

**Append-only logs.** A canonical LOG file is never archived, and no LOG file is edited after the fact. Append-only governs an entry's content, not its position: once written, an entry is never revised, split, or merged. New entries go at the end. The one exception is folding another log into this one, which places that log's entries whole and in date order among the existing ones, so the last separator still holds the most recently written entry. A fold may rewrite an entry's references to work-item files that are retiring and drop its pending-closeout block; it changes nothing else in the entry.

**File naming.** Every mini-brain filename carries the `MBT` lobespace — shared docs as `MBT_*`. Four files are exempt: `CLAUDE.md` (entrypoint), `AGENTS.md` (sends agents that read only it to the entrypoint), and the repo artifacts `README.md` and `CONTRIBUTING.md`. New top-level knowledge files must include the lobespace; files in `archive/` keep whatever basename they were retired under.

**One namespace across two repos.** `MBT_` names one namespace that spans this repo and the product. Each `MBT_` name lives in exactly one of the two and is never duplicated in the other.

**Version header.** Every top-level file except the version-exempt ones opens with `> V<N>, YYYY-MM-DD.` — version and date, nothing else. **No change note in this line.** If a file needs a description, put it on its own line below. Bump `<N>` by one on each substantive edit (numerically — `V10` > `V9`) and set the date. This applies to `CLAUDE.md` itself. Exempt: `AGENTS.md`, `README.md`, `CONTRIBUTING.md`, `*_LOG.md` files, and `*_BURNDOWN.md` checklists.

**Classify every edit:** *editorial* (typos, rewording) — edit in place, don't bump `<N>`, may update date; *substantive* (facts, decisions, scope, structure) — set the version line to `> V<N+1>, <today's date>.` No change note in the line.

**Cross-references.** Cite the canonical top-level filename, never a retired copy in `archive/`. Reference a file's *own* sections freely (`§1.3`, "the roadmap below"); reference *another* file by name plus a short description of what's there — never by its section or phase number ("the comparative analysis in `MBT_RESEARCH.md`," not "`MBT_RESEARCH.md` §1.3"), since foreign section numbers break silently on renumber.

**Orthogonal content.** Each mini-brain file covers one concern and tells its own story — keep that orthogonality in the *prose*, not just the file boundaries. A file describes its own subject, never narrating a sibling's (`MBT_SCOPE.md` states the problem; it does not recap the design that lives in `MBT_APPROACH.md`). State how a file relates to the others **once**, in its header (the read index carries the rest), then stop pointing sideways. Wiki-style narration — "the data for this lives in X," "see Y," "as covered in Z" — keeps creeping in; resist it: it re-couples files that are meant to stand alone, and rots when they move. Keep the content (what a file holds and why); cut the running navigation.

**Declared reference exemptions (project-specific).** Orthogonality is the default and it is strict: a knowledge file names *no* sibling and stands on its own. A cross-file reference exists only where this project **declares an exemption**, and an exemption may only point *upstream* — toward the problem a file serves — never *downstream* toward how it was built. A downstream reference is never exemptable; that invariant is what stops content bleed-through. This mini-brain's core knowledge docs and their declared exemptions:

| File | May reference | Why |
|---|---|---|
| `MBT_SCOPE.md` | *none* | The problem statement stands alone; it precedes and outlives any design. |
| `MBT_APPROACH.md` | `MBT_SCOPE.md` only | A design legitimately responds to the problem it addresses — never reaching down into implementation. |
| `MBT_FINDINGS.md` | *none* | Implementation decisions that did *not* shape the approach — downstream of it, so nothing upstream to cite. |
| `MBT_ENFORCED_TERMS.md` | *none* | Each entry stands alone; it disambiguates without referencing the term's home document. |
| `MBT_EXAMPLES.md` | *none* | A list of brains and their provenance stands alone; it needs no procedure to be read. |
| `MBT_RESEARCH.md` | *none* | The conclusions the two registries feed upward into; naming them would reach *downstream*, and it needs nothing else. |
| `MBT_COMPARABLES.md` | `MBT_RESEARCH.md` only | A fact registry serves the conclusions it feeds; it may name that conclusions doc (upstream) and nothing else. |
| `MBT_BIOLOGY.md` | `MBT_RESEARCH.md` only | Same, for the cognitive-science grounding registry. |

Absent a declared exemption, full orthogonality holds. A permitted reference still follows the Cross-references rule above — cited by name, never by section number. Naming the product's files is not a cross-reference: they are the code this brain describes.

**Maintenance-doc boundaries.** Any procedure that appends a log entry cites `MBT_SESSION_CLOSEOUT.md` as the entry-format authority. Beyond that, `MBT_DREAM_CYCLE.md` references no other maintenance doc, and `MBT_WORK_SETUP.md` / `MBT_WORK_CLOSEOUT.md` are bookends that may reference each other, but only by that relationship. `MBT_SESSION_CLOSEOUT.md` governs the session logs — `MBT_LOG.md` and any open work item's working log — and references nothing else. `MBT_TESTING.md` runs the product's procedures as tests; beyond the product's files it references only `MBT_EXAMPLES.md` and the scripts in `tests/`. Naming the knowledge files a procedure operates on is inherent to being a procedure, not a cross-reference.

**No harness memory.** Don't use the persistent memory feature. All persistent project information belongs in the markdown files in this directory.

---

## Writing Docs and Instructions

All prose in this repo is read like a proof or a program, not a wiki. The reader is adversarial: every token is load-bearing, and every spare one is a question they must resolve.

- **Say it once.** Each fact has exactly one home. Duplication isn't emphasis; it's a second copy to keep in sync and a sign the first statement didn't land.
- **Define before use.** Introduce a thing where the reader first needs it, in dependency order. A forward pointer ("see Step 3", "as below") means the content is in the wrong place — move it, don't link to it.
- **No sideways narration.** A section explains its own subject and never recaps a sibling section's; pointing (`§3`) is fine, restating is not. Across files, the Orthogonal content rule above governs.
- **Every claim earns its keep.** State only what the reader must act on and what you could defend if challenged. Decorative rationale, benefits, and history are cut.
- **Resolve, don't provoke.** A detail that raises a question it doesn't answer is a net loss — prefer omission to a half-explanation.
- **One job per sentence.** A sentence tells the reader what to do or explains why — never several at once.
- **No hard wrapping.** Write each markdown paragraph and list item as one continuous line and let it soft-wrap; manual line breaks inside a paragraph make noisy diffs and fight reflow.
- **Enforced terms bind.** When this brain declares enforced terms, use each listed term in its enforced sense or choose a different word.

When a line's contribution isn't obvious, it isn't contributing — delete it.

---

## Commit Conventions

**No AI attribution.** Commits carry no `Co-Authored-By: Claude` (or similar) line, and PR descriptions carry no "Generated with" footer — AI authorship is assumed for this repo and isn't stamped per commit or PR. This overrides any harness default that would append one.

**Straight to `main`.** Session closeouts and other edits commit and push straight to `main`. A work-item closeout goes through a PR against `main`.
