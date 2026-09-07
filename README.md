# my-skills

Personal collection of Claude Code / agent skills.

## Layout

| Directory | Contents |
|---|---|
| `engineering/` | Diagnosis, TDD, refactoring, web app testing |
| `productivity/` | Interviewing, writing, skill authoring |
| `personal/` | Tied to my own setup, not general purpose |
| `misc/` | Repo scaffolding and guardrails |
| `deprecated/` | Kept for reference, not installed |

Most skills originate from Matt Pocock's collection (see
`mattpocock-skills-intro.md`); the bundled `LICENSE.txt` files carry their
original terms.

## Not included

`gamestudio/` is a third-party skill maintained in its own repository and is
excluded here. To restore it:

```bash
git clone https://github.com/guangyuspace/codex-gamestudio-skill gamestudio
```

## Installation

Skills are used via directory junctions from `~/.claude/skills` and
`~/.agents/skills` pointing into this checkout, e.g.:

```
mklink /J "%USERPROFILE%\.claude\skills\diagnose" "C:\_work\AI_Work\Skills\engineering\diagnose"
```
