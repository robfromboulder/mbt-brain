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

---

# Run the split's acceptance tests in Codex, OpenCode 2 and Linux (2026-11-13)

**Session ID**: `479b3910-a2f2-49fe-9c5a-77f8d0ad0c69`

Rob, with Claude as co-author and coordinator, asked for the second half of the split's acceptance run: Codex and OpenCode 2, both now installed, plus Linux through OrbStack. Claude settled the open questions with Rob up front, then ran three tracks unattended against PR #32's head, `0ca64ac`. Every scenario run passes, scenario 6 included, so the Codex and OpenCode 2 gate before tagging is met; no toolkit defect turned up, and PR #32 gained no commits.

## Decisions

All Rob's, on Claude's recommendation, before the run:

- **Linux covers agents too.** Besides the shell checks, Codex and OpenCode 2 ran scenarios 2 and 16 in a Debian container, against the testing doc's host-only rule.
- **Scenarios:** 2, 5 and 16 in each harness, 12 in Codex, one run of 6 in Codex, and 3 in OpenCode to build the brain scenario 5 needs.
- **Medium reasoning effort, by flag,** since Codex's config sets low; the config stayed untouched. Both harnesses ran gpt-6.1-sol through LiteLLM.
- **Writes allowed unattended:** this log and the burndown, pushed to `main`, and fix commits to PR #32 only for a clear toolkit defect.

## The run

- Three forked Claude tracks ran at once: Codex on the host, OpenCode 2 on the host (plus scenario 5 in Codex), and Linux. Each ran its tested agents one at a time.
- Linux: the shell checks passed 67 of 67 on macOS and in Debian. In the container, Codex's sandbox can't create a namespace, so its runs bypassed the sandbox with the container as the boundary. OpenCode 2 isn't on npm, so it came as an unverified binary from its site. It also ignored the mounted `auth.json`, so the key reached it as an environment reference in its config.
- Scenario 16 passed in all four harness-and-platform pairs, each entry carrying its harness's own session variable rather than the inherited one.
- Scenario 6 in Codex: the first report listed only the hook paragraph that is one behind and the planted own edit. The walk-through brought every other file current, and the report-only recheck changed nothing.
- Scenario 3 asked the premises in a round of their own, so the blind-create failure from the previous run did not recur.

## What didn't work

- The first scenario 12 run at the release tag stopped at its first heredoc. zsh writes here-document temp files under `TMPPREFIX`, which defaults to `/tmp/zsh`, and ignores `TMPDIR`. The testing doc's Codex setting blocks `/tmp`, and the dream cycle's no-retry rule then ended the cycle, as it should. Setting `TMPPREFIX` into the run's temp directory fixed the reruns. Scenario 16's agent hit the same block but worked around it.
- The first scenario 12 run at the commit under test hung for over two hours after a completed file search, and its track sat waiting on it. Claude noticed when Rob asked how the tracks were doing, had it killed, and reran both halves under a watchdog. The cause is unknown.
- Forked tracks couldn't write their report files, so the coordinator saved them.

## Lessons

- An unattended run needs a stall watchdog on every tested agent from the start, not after a hang.
- Codex's sandbox without network makes check and the dream cycle report external facts as unverified. Both said so rather than guessing.

## Open

- The testing doc's Codex command should set `TMPPREFIX` beside `TMPDIR`.
- Scenario 12 says the leakage check flags the `working/` citation, but the dream cycle replaces it with the cited content while the file still exists. The scenario should say that.
- Whether OpenCode's medium variant took effect through LiteLLM is unconfirmed.
- Create wrote two log entries with one session ID, after the seed and after stage 2, and doesn't say which is right.
- Scenario 3's agent never raised the kilns as a premise; it surfaced only through the catch-all constraints question.
- The scenario 12 head run proposed two instruction changes: separate required filename references from naming-exemption examples, and separate installed-release checks from upstream-main checks.
- Scenario 2's `@`-menu selection in OpenCode 2 is still Rob's manual step.
