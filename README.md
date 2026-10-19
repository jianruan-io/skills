# skills

Claude Code process skills I use every day — repeatable loops with gates, not one-off tools.

Agents now do most of the making. The human's job is to **prove it's right** — so every skill here is built around a gate you can check, not a promise you have to trust.

## Skills

### thinking

| Skill | What it does |
|---|---|
| [first-principles](skills/thinking/first-principles/SKILL.md) | Lock the problem, question every belief, keep only bedrock facts, build the solution from those |
| [delete-algorithm](skills/thinking/delete-algorithm/SKILL.md) | Question requirements, delete, then simplify — and flag a 0% deletion rate |
| [new-skill](skills/thinking/new-skill/SKILL.md) | Turn any action or process into a skill file |

### engineering

| Skill | What it does |
|---|---|
| [e2e-verify](skills/engineering/e2e-verify/SKILL.md) | Prove a user goal works end to end — no mocks, status derived from real runs, never typed |

## Install

Copy any skill folder into `~/.claude/skills/` (every project) or `.claude/skills/` (one project):

```bash
git clone https://github.com/jianruan-io/skills.git
cp -r skills/skills/thinking/first-principles ~/.claude/skills/
```

Then type `/first-principles` in Claude Code, or just describe the problem and let it trigger.

## License

[MIT](LICENSE)
