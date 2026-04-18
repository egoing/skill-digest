# skill-digest

> Analyze any Claude Code / Claude Agent skill and render it as a standardized, cognitive-load-optimized digest.

Point it at a skill name, a local path, or a GitHub URL — it reads the `SKILL.md`, classifies the skill, extracts only the truly load-bearing parts, and prints a compact digest you can absorb in under a minute.

## Why

Reading a stranger's `SKILL.md` cold is expensive — you don't know which lines are the *actual* contract and which are implementation noise. `skill-digest` does the extraction for you:

- **One bold `Key:` sentence** — what this skill automates or prevents. The one thing you must remember 10 minutes later.
- **1–3 pain scenarios** — when would I reach for this?
- **Workflow in 3–7 steps** — no sub-bullets, no prose.
- **`⚠ Watch Out` / `💡 Tips`** — user-facing only, and only when the skill explicitly documents them. No LLM-invented warnings.
- **Progressive disclosure** — examples, related skills, and metadata hide inside a `<details>` block. The first menu is a single `[1] Learn more`.

Every `##` heading is prefixed with `◆` so a digest block is visually distinguishable from surrounding conversation.

## Installation

Clone into your Claude Code skills directory:

```bash
git clone https://github.com/egoing/skill-digest.git \
  ~/.claude/skills/skill-digest
```

Claude Code auto-loads skills under `~/.claude/skills/`. No other setup required.

## Usage

```
/skill-digest <target>
```

### Input forms

| Form | Example |
|------|---------|
| Local skill name | `/skill-digest recap` |
| Local path | `/skill-digest ~/.claude/skills/foo/SKILL.md` |
| GitHub full URL | `/skill-digest https://github.com/user/repo/blob/main/SKILL.md` |
| GitHub short form | `/skill-digest user/repo` |
| GitHub collection | `/skill-digest user/repo` (multiple `SKILL.md` → collection digest) |

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
- **Localization.** If the user's language is not English, headings translate automatically while the `◆` prefix stays language-independent.

## Requirements

- Claude Code (skills feature enabled)
- Read / Glob / Grep tools
- `WebFetch` for GitHub URL inputs

## License

MIT — see [`LICENSE`](LICENSE).

## Author

[@egoing](https://github.com/egoing)
