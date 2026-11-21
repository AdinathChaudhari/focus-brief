# Tests: five prompts, three setups

Run on 2026-11-21. Each prompt went to a fresh headless session (`claude -p`) in an empty folder. All three setups used the same flags: Skill tool only, no MCP connectors, and the plugin loaded with `--plugin-dir`. The owner's global CLAUDE.md applied in all three.

| Setup | What changes |
|---|---|
| **Before** | The previous always-on output style (i-have-adhd's rules) |
| **Skill** | The same, plus `/focus-brief` typed before the prompt |
| **Style** | The focus-brief output style instead, with no slash command |

## The five prompts

1. Four unrelated asks in one dictated message
2. A pasted code-review comment with no question
3. "This has got way too complicated" after a long git rebase
4. "Build me a small CLI that sorts my screenshots"
5. A small chat question (git fetch vs pull)

## Results

Each cell counts the prompts that passed that check. Cost and the build gate only count when they apply.

| Check | Before | Skill | Style |
|---|---|---|---|
| Decision or action on the first line | 5/5 | 5/5 | 5/5 |
| Five steps or fewer | 5/5 | 5/5 | 5/5 |
| A time is given | 4/5 | 5/5 | 5/5 |
| **Kickoff fail rule (all three checks above)** | **4/5** | **5/5** | **5/5** |
| Parked threads and state shown | 1/5 | 5/5 | 5/5 |
| Cost stated when work is proposed (4 prompts) | 0/4 | 2/4 | 3/4 |
| Build gate held: restated, waited for "go" (prompt 4) | no, wrote the whole tool | yes | yes |
| Bare paste gets "Do you want X?" first (prompt 2) | no, wrote the fix | yes | yes |

The kickoff fail rule (decision first, five steps or fewer, a time given) barely separates the setups: the old setup already does those things. The difference shows in the focus-brief-only checks: the park list, cost, the build gate, and the bare-paste question.

## Finding: the skill does not load by itself

In 5 headless runs, Claude never loaded the skill on its own, even though its description says "use on every message". The same was true of the i-have-adhd skill. Always-on shaping comes from an output style. So focus-brief ships in two forms:

- `skills/focus-brief/SKILL.md`: load it with `/focus-brief`.
- `output-styles/focus-brief.md`: always on. Tested above as "Style".
