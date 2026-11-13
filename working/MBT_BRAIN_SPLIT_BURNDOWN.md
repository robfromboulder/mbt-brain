# Brain Split: Burndown

Everything between working code and a merged PR.

## Routine

* commit and push the finished change on the work branch
* create draft PR
* review code changes
* full rebuild and automated retest on PR code
* review PR description against final code one last time
* request feedback from reviewers
* respond to review feedback
* automated checks — see `MBT_BRAIN_SPLIT_CLAUDE.md`
* manual checks — capture steps in `MBT_BRAIN_SPLIT_TESTING.md`
* closeout notes — <deviations from the standard flow, and any canonical claim this work item reverses — see `templates/WORK_CLOSEOUT.md`>

## Custom

* rehearse the split with the drafted files; gate the product PR on the report
* push the import to `mbt-brain`
* docs — README and CONTRIBUTING in both repos
* run the acceptance scenarios in Codex and OpenCode 2 before tagging — at least 2, 5, 12, 16 and one run of 6, which cover loading through `AGENTS.md`, installs at `~/.agents/skills`, the path rule as each harness reads it, and each harness's session variable
* tag the first product release after the merge
* update a real 0.73 install with the unchanged README line
* repoint local clones and worktrees: the brain beside the toolkit checkout
