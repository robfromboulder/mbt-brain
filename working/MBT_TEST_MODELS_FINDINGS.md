# Test Models: Decision Record

## Problem

A tested agent runs on its harness's default model, which is whatever the runner's machine last set. In BRAIN_SPLIT's acceptance run every Claude Code agent inherited the runner's saved default, an Opus-class model, and about thirty headless runs drew the runner's subscription usage from 40–50% to 93% of its weekly limit. The report records each harness's version but not its model, so two runs of the same scenario can differ for a reason the report never shows, and OpenCode 2's default on a given machine is unknown.

## Preferred approach: name the model per run, record it in the report

Each harness's headless command in `MBT_TESTING.md` passes a model flag, and the report header lists the model each harness ran.

| Aspect | Detail |
|---|---|
| Choice | The runner picks the model per run |
| Record | Model per harness in the report header |
| Default | Open: see the plan's open issues |

- **Cost** — a cheaper model serves most scenarios, which test whether instructions can be followed rather than how far a model can reason.
- **Comparability** — a result is read against the model that produced it.
- **Machine independence** — a run no longer depends on the last model a harness was set to.

## Alternatives considered

- **Leave each harness on its default.** Rejected: the cost and the unrecorded variable are the problem.

## What this doesn't solve

- A run's cost still scales with how many scenarios it picks.
