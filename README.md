# skill-digest

> Analyze any skill written to the [Agent Skills](https://code.claude.com/docs/en/skills) format and render it as a standardized, cognitive-load-optimized digest.

Works in any Agent Skills-compatible runtime — [Claude Code](https://claude.com/claude-code), [skills.sh](https://skills.sh), or any other host that reads the `SKILL.md` format. Point it at a skill name, a local path, or a GitHub URL — it reads the `SKILL.md`, classifies the skill, extracts only the truly load-bearing parts, and prints a compact digest you can absorb in under a minute.

## Why

Reading a stranger's `SKILL.md` cold is expensive — you don't know which lines are the *actual* contract and which are implementation noise. `skill-digest` does the extraction for you:

- **One bold `Key:` sentence** — what this skill automates or prevents. The one thing you must remember 10 minutes later.
- **1–3 pain scenarios** — when would I reach for this?
- **Workflow in 3–7 steps** — no sub-bullets, no prose.
- **`⚠ Watch Out` / `💡 Tips`** — user-facing only, and only when the skill explicitly documents them. No LLM-invented warnings.
- **Progressive disclosure** — examples, related skills, and metadata hide inside a `<details>` block. The first menu is a single `[1] Learn more`.

Every `##` heading is prefixed with `◆` so a digest block is visually distinguishable from surrounding conversation.

## Installation

### Recommended: `npx skills` (host-neutral)

Use the [`skills`](https://www.npmjs.com/package/skills) CLI by Vercel Labs — it auto-detects your agent's skills directory and installs there:

```bash
npx skills add egoing/skill-digest
```

Target a specific agent with `-a`:

```bash
npx skills add egoing/skill-digest -a claude-code
npx skills add egoing/skill-digest -a opencode
npx skills add egoing/skill-digest -a codex
# See `npx skills --help` for the full agent list
```

### Alternative: direct git clone

`skill-digest` is a plain Agent Skills package — the repository root is the skill directory, so you can also clone it into whichever directory your host agent scans for skills.

**Claude Code:**

```bash
git clone https://github.com/egoing/skill-digest.git \
  ~/.claude/skills/skill-digest
```

For other hosts, consult your runtime's docs for the skills directory and clone into it. No build step, no manifest conversion.

## Usage

How you invoke a skill depends on your host. Common patterns:

- Slash command: `/skill-digest <target>` (Claude Code)
- Natural language: `run skill-digest on <target>`
- Programmatic: invoke via your host's skill-invocation API

### Input forms

| Form | Example |
|------|---------|
| Local skill name | `skill-digest recap` |
| Local path | `skill-digest <skills-dir>/foo/SKILL.md` |
| GitHub full URL | `skill-digest https://github.com/user/repo/blob/main/SKILL.md` |
| GitHub short form | `skill-digest user/repo` |
| GitHub collection | `skill-digest user/repo` (multiple `SKILL.md` → collection digest) |

### Follow-up actions

After the first digest you'll see `→ [1] Learn more`. Picking `[1]` expands to:

- `🧩 Quiz` — 3 questions (recognition / workflow ordering / documented gotcha)
- `🔬 Deep Dive` — focused explanation of the 2–3 most non-obvious behaviors
- `🎯 Practice` — a scenario where this specific skill is the right tool
- `⚖️ Compare` — side-by-side against another installed skill
- `🗺️ Skill Map` — where this skill sits relative to your other skills

## Design principles

- **Documented-only, no inference.** `Watch Out` and `Tips` include only hazards or shortcuts the skill's own docs explicitly name. An empty section is better than an invented one — LLMs cannot reliably infer real hazards from reading a workflow, and fabricated warnings erode user trust.
- **User perspective, not agent perspective.** Internal execution rules such as "sub-agent must not do X" never appear in the digest — they are invisible to the user and belong in the skill's source.
- **Progressive disclosure.** The first view is deliberately narrow (five blocks + one menu item). Depth is one click away.
- **Host-neutral.** No assumption about Claude Code specifically; any Agent Skills runtime that supplies Read / Glob / Grep / WebFetch can run it.
- **Localization.** If the user's language is not English, headings translate automatically while the `◆` prefix stays language-independent.

## Requirements

- A runtime implementing the [Agent Skills spec](https://code.claude.com/docs/en/skills)
- Tools: `Read`, `Glob`, `Grep`, `WebFetch` (for GitHub URL inputs)

## License

MIT — see [`LICENSE`](LICENSE).

## Author

[@egoing](https://github.com/egoing)
