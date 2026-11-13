# Brain Split: Session Log

Running log of implementation sessions. Each session appends after a `---` separator.

The first entry, and every one after it, follows the entry format and content rules in `MBT_SESSION_CLOSEOUT.md` — that spec is the authority, not the prior entry, since a fresh LOG file has none to copy.

---

# Split the brain out of the toolkit and run its acceptance tests (2026-11-12)

**Session ID**: `9ce5754c-4439-4469-aae2-5727256aff72`

Rob, with Claude as co-author, began by asking where to list the toolkit beyond LinkedIn. The plugin directory's per-file limit exposed that every install ships the toolkit's own brain, and the session turned into this item: across 2026-11-11 and 2026-11-12 it weighed four ways to keep the brain out of installs, chose moving it to its own repo, settled the split's decisions, imported the brain into `mbt-brain`, opened toolkit PR #32 as a draft, and ran the acceptance tests overnight in Claude Code. Every scenario the machine could run passes; merging and tagging wait on Rob's review and on runs in Codex and OpenCode 2.

## From install clutter to a split

- DISTRIBUTION's research found `MBT_LOG.md` at 369 KiB, over Anthropic's directory's 256 KiB per-file limit, and a plugin install copies the whole repo. Rob asked whether a GitHub release could filter it. Claude said no, since every channel installs from a git ref, and proposed a generated release branch built from an opt-in manifest and set as the default branch.
- Rob widened the problem: the dream-cycle and example docs are clutter too, the one-line install per harness must survive, and the list of what ships should be opt-in so it can't drift. He noted viewzoo and viewmapper already depart from a `main`-first workflow, and floated a separate brain repo as drastic. Claude laid out the release branch, the split, a subdirectory and a release zip, and recommended the release branch. Rob asked about GitHub Packages; Claude rejected it: npm's registry needs a token even for public installs, its container registry needs extra tooling, and no channel installs from a package.
- Rob set the release convention: merging to `main` means ready for the next release, every change is testable on its branch, and rebase merges carry that tested state.
- Claude scaffolded RELEASE_BRANCH. Writing it found that `check` reads template history from the install, so release commits would need a second parent on `main`.
- Rob objected that a manifest, a script and special commits, all specific to this repo, weaken the toolkit as an example of its own pattern. Claude proposed the split as the pattern's own answer, its second principle being that a brain is not shipped with its product. Rob proposed the opposite instead: a generated product-only skills repo. Claude found it needs `git filter-repo` to keep check's history, a repointed origin and force-fetched tags for existing installs, and a second front door. Rob ruled out both `filter-repo` and two front doors.
- Rob asked whether `archive/` and `working/` actually cause harm. Claude: no, only the logs (size) and the instruction-shaped maintenance docs do, and offered log rotation as the narrow fix.
- Rob came around to the split: the skill is now the only way to use the toolkit, and the toolkit being a brain made sense only while the brain was the runtime surface. RELEASE_BRANCH, never committed, was deleted, and BRAIN_SPLIT opened with the file map. The repo was named `mini-mbt-brain`, then `mbt-brain` once Rob created it.
- Rob remarked that separate repos won without his steering hard. Claude recorded the reason in FINDINGS as one repo per lifecycle.

## Decisions

All Rob's, on Claude's recommendation unless noted:

- **Instruction files mirror the exemplars,** lamarr, nebula, viewzoo and viewmapper for the toolkit and their brains for `mbt-brain`; rules both repos share are repeated in each, not templated (Rob). The hook goes in `AGENTS.md` per the template, though the exemplars still hold theirs in `CLAUDE.md`.
- **The path rule names the files the install holds,** not every `MBT_*.md`.
- **Product files drop their version headers** (Rob: they are skill material, not brain material); templates keep their `V1` line (Claude).
- **The brain commits to `main`; work-item closeouts go by PR,** as brains usually do (Rob).
- **`MBT_` is one namespace across both repos, and each name lives in exactly one** (Rob, for continuity); no file is renamed.
- **The hook template's back-reference rule is keyed to the brain repo's files, not their prefix,** fixed in the template rather than edited locally.
- **The dream cycle is rebuilt from the template,** with product problems flagged rather than fixed. Mid-implementation, asked what survives of the toolkit's own Phase 2, Rob kept the product-consistency check and the self-check, flag-only.
- **Session closeout takes back the template's branch signal;** overnight, Rob approved taking the template's text for two further spots as well.
- **The reference rules for product files move to the toolkit's `CLAUDE.md`.**
- **For the overnight run** (Rob): fixes to PR #32 only as new commits, never a force-push; `mini-subtitle-wizard-brain` cloned into scratch; the tested agents' allow-list widened only where a block kept a scenario from its intent.

## Research before implementing

- A scripted upgrade, from a 0.73 install to a product-only tag with the unchanged install line, went from 102 tracked files to 39, kept check's template history reachable, and stopped with git's error on an install with a local edit to a moved file.
- A headless report-only check of a scratch split confirmed the narrowed path rule routes the brain's own reads to the brain. It also found the hook rule's collision with the one namespace, the dream cycle missing the template's release check and Scope accuracy, and session closeout missing the branch signal.
- Rob asked for four read-backs of PLAN and FINDINGS. Each caught claims of Claude's that had overreached: exemplar section shapes stated without reading them, a step order that would have run check through the old path rule, `check` behaviour asserted without reading the procedure, the brain's `AGENTS.md` listed as new when it already existed, and inferences recorded as Rob's decisions until marked as Claude's.

## Implementation

- The import landed at toolkit `d988039`, 80 files unchanged. Claude rewrote the entrypoint, instantiated work setup and closeout from the templates, reconciled the APPROACH and FINDINGS reversals, and pointed the testing doc at the sibling checkout. An IDE folder swept into one commit was amended out before anything was pushed. Rob asked for the toolkit's license in the brain.
- The toolkit's `brain-split` branch got four commits, opened as draft PR #32 once a rehearsal with the real files gated it. Doing the real edits locally and unpushed meant the rehearsal tested the actual files rather than a second copy.
- The shell checks gained a case: an install of the newest release, updated to the commit under test, holds exactly the files that commit tracks.
- The rehearsal scored the dream cycle as the one partial principle. Acceptance scenario 13 dry-runs that file, so the rebuild moved ahead of the acceptance run, against the PLAN's original order.

## Acceptance run

Run overnight in Claude Code 2.1.322 only, with the Linux half in Docker; Codex and OpenCode 2 were not installed.

- Every scenario the machine supports passes, scenario 6's upgrade from 0.73 included. The reworded hook rule shows as the one paragraph behind on a 0.73 brain, and is invisible on brains that had edited theirs.
- Two failures predate this item: blind create never asks about each premise on its own, and check's temporary copies land in `/tmp` rather than `$TMPDIR`.
- A toolkit clone is no longer a brain, so `check` now stops on it. Two scenarios built on checking a toolkit copy were retargeted rather than the procedure changed.
- The allow-list gained `printenv` (Claude Code refuses a command that expands the session variable), `mktemp` and `mv`.
- The dream-cycle dry run found the rebuild's two gaps, a refused read of the toolkit clone and which branch counts as `main`. Both were fixed; the run's other notes predate the rebuild.
- Usage limits cut three batches short. Each resumed from the scratch state without repeating finished work. Tested agents cost about $44.

## What didn't work

- The first import copied nothing, because zsh does not word-split an unquoted variable.
- In scenario 3, answering the premises before the agent asked them hid whether it asks each on its own. The re-run answered only what each turn asked.
- The first stage-1 fixture deleted a regular template bullet as if it were staged text, so check rightly reported it missing.

## Lessons

- Several scenarios assumed what the toolkit is (a brain) without saying so. Changing that broke tests, not procedures.
- A rehearsal of the real files, unpushed, is a gate. A rehearsal of drafts is a second thing to keep in sync.

## Open

- Run the Codex and OpenCode 2 scenarios before tagging, then merge and tag with the release-notes caveat on the hook.
- Whether APPROACH's dogfooding section keeps its history paragraph or becomes present-tense rationale.
- Check 10's skip for a toolkit target now only serves pre-split clones.
- The dream-cycle dry run's notes that predate the rebuild.
- Naming `$TMPDIR` in check's temporary-directory step.
- The example list's virtual-view-brain clone-path entry disagrees with both the agent and the grader.
- `/private/tmp/drift.buoL` matched no surviving transcript and was left in place.
