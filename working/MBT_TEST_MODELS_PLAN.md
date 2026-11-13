# Test Models: Implementation Plan

## Objective

Make the model each tested agent runs on a choice the acceptance-test runner makes and records, rather than whatever the harness's default happens to be on the machine at hand. Each harness's headless command names a model, and the report header states the model each harness ran. No change to which scenarios run or how they are graded.

## Context

See `MBT_TEST_MODELS_FINDINGS.md` in this directory for the decision record and alternatives considered.

Key references in the owning lobe:
- `MBT_SCOPE.md` — problem space this fits into
- `MBT_FINDINGS.md` — relevant prior decisions
- `MBT_APPROACH.md` — design constraints

## What changes

- `MBT_TESTING.md` — each harness's headless command in the run section names a model; the probe or select step settles which model each run uses; the report header records it.

## Interaction with existing code

| Existing code | Interaction | Risk |
|---|---|---|
| `MBT_TESTING.md` cost figures | Quoted for scenarios run in Claude Code on its default model | They no longer hold for a cheaper model |
| `tests/` scripts | Run no model | None |

## Testing approach

No automated test: the scripts in `tests/` run no model and stay green. Manual steps live in `MBT_TEST_MODELS_TESTING.md`.

## Implementation sequence

1.

## Scope boundary — what this does NOT include

- Which scenarios run, or the selection order.
- Grading, or the scenarios' expected outcomes.
- A cost budget or cost controls beyond choosing the model.

## Open issues

1. **Which model a release-gating run uses**, against a smoke or iteration run. Rob's view: a Sonnet-class model suits testing better than an Opus-class one; whether the run that gates a release should use the model maintainers dream and develop in is unsettled.
2. **The flags.** `claude -p --model`, `codex exec -m` and `opencode run --model` are believed to exist; confirm each against the installed harness before writing them in.
3. **OpenCode 2's default.** Which model `opencode run` uses without a flag is unknown; it may come from its config or from the last model selected.
4. **Alignment.** BRAIN_SPLIT has unmerged edits to `MBT_TESTING.md`, the allow-list and two retargeted scenarios among them; build on them. MULTI_LOBE_DREAM expects to change `MBT_TESTING.md`'s scenarios; neither item's change should rewrite the other's lines.
