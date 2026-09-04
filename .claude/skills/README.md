# SoundBox skills — for your Claude

Three skills that teach Claude how to help build music in **SoundBox**. Hand them to your own Claude and it will know the project.

| Skill | Use it for |
|---|---|
| **soundbox-scriptlet** | Describe a musical idea → get pasteable scriptlet text (melody or drums) for SoundBox's Scriptlet box. |
| **soundbox-arrange** | Translate production intent (mood, sections, bridges, filter/delay moves) into SoundBox features. |
| **soundbox-dev** | Modify the `SoundBox.html` source safely — engine map + the verify-before-you-claim workflow. |

## Install (Claude Code)
Copy the three folders into your project's `.claude/skills/` directory (or your user-level `~/.claude/skills/`):

```
your-project/
└── .claude/
    └── skills/
        ├── soundbox-scriptlet/SKILL.md
        ├── soundbox-arrange/SKILL.md
        └── soundbox-dev/SKILL.md
```

If you clone this repo, they're already in the right place — open the repo folder in Claude Code and the skills are available. Claude picks the right one automatically from its description; you can also invoke one by name (e.g. `/soundbox-scriptlet`).

## Not using Claude Code?
The `SKILL.md` files are just Markdown. Paste the relevant one into the conversation as context ("Here's how SoundBox scriptlets work: …") and Claude will follow it.

The full notation spec (with the open design questions) lives alongside the app.
