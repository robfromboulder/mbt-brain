# Distribution: Session Log

Running log of implementation sessions. Each session appends after a `---` separator.

The first entry, and every one after it, follows the entry format and content rules in `MBT_SESSION_CLOSEOUT.md` — that spec is the authority, not the prior entry, since a fresh LOG file has none to copy.

---

# Survey listing channels and open DISTRIBUTION (2026-11-12)

**Session ID**: `9ce5754c-4439-4469-aae2-5727256aff72`

Rob, with Claude as co-author, asked where a free, Apache-2.0 skill could be listed to reach agent users beyond one-off LinkedIn posts, given that Agensi had emailed about listing MBT. The session surveyed the channels, opened this item from the results, ran a second research round, and found the item depends on BRAIN_SPLIT before anything is submitted.

## Turn by turn

- Claude surveyed Anthropic's plugin directory, skills.sh, the awesome lists, Agensi and the crawled registries, reading each channel's own pages because the search summaries disagreed. It recommended the plugin directory, skills.sh, hesreallyhim's awesome-claude-code and travisvn's list once the repo has ten stars, and GitHub topics.
- Rob asked to seed a work item from that. Claude opened DISTRIBUTION.
- Rob pointed out that the earlier rejection of plugin packaging never weighed the registries a plugin could be published into. The rejection was reopened with reach as a third factor beside command syntax and update path.
- The second round settled most of what the rejection had left open. A root `SKILL.md` loads as a single plugin skill, and a plugin install copies the whole directory, so the path rule holds; a plain skill and a plugin of the same name coexist; and versions pin. What remains is the namespaced command. skills.sh lists skills by install telemetry, finds a root `SKILL.md` and installs the whole repo, pinned by tag. OpenAI keeps one directory for ChatGPT and Codex. Gemini CLI and Cursor were deferred: listing a harness promises brains load there.
- Agensi lists free skills, but delivers them only through its connector behind an account, its buyer terms forbid the redistribution Apache-2.0 grants, and every release is a fresh upload. Claude recommended declining.
- `MBT_LOG.md` is over the directory's per-file limit. That led to BRAIN_SPLIT, whose own log records the design.

## Consequence of BRAIN_SPLIT

- DISTRIBUTION depends on BRAIN_SPLIT: its open issue on the per-file limit resolves there, and submissions follow the first release after the split.

## Open

- The plugin decision and the Agensi reply are Rob's.
- What a root-`SKILL.md` plugin's command reads, bare and namespaced, has to be tested.
- Whether OpenAI's directory takes a skill-only plugin is unconfirmed.
