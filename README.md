# mbt-brain 🧠

This [mini-brain](https://github.com/robfromboulder/mini-brain-toolkit) allows all mini-brain-toolkit knowledge and history to be used from any coding-agent session, and provides standard workflows to maintain and improve this knowledge over time.

This is not a "second brain" with the goal of capturing all mini-brain-toolkit knowledge -- this mini-brain only maintains information that cannot be derived from the codebase.

## Knowledge files

All knowledge is stored in Markdown files. Each file captures an orthogonal dimension of knowledge about mini-brain-toolkit: its scope, technical approach, work in progress, session history, and so on.

CLAUDE.md provides a read index so that the coding agent can discover and apply knowledge that is relevant to the active chat session.

Knowledge files in this mini-brain are `MBT_`-namespaced. This convention lets you load more than one mini-brain into the same coding session without filename collisions or confusion during updates.

## Usage

#### 1. Use mini-brain-toolkit knowledge from a project repo or any of its worktrees:

> Read CLAUDE.md in the mbt-brain clone beside this repo's main checkout, for instructions

#### 2. Save notes about this session:

> Read MBT_SESSION_CLOSEOUT.md and append this session to the logs it routes to
