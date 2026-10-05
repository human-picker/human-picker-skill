# human-picker skills

A bundle of Claude Code / Agent Skills for asking humans to make small, reversible visual and audio decisions during AI-assisted development.

中文文档: [README_zh.md](README_zh.md)

Each skill in this repository targets a single picker tool. They are independent — install only the ones you need.

## Skills included

| Skill | CLI | When to use |
|---|---|---|
| [`svg-picker/SKILL.md`](svg-picker/SKILL.md) | `pip install svg-picker` | Pick a UI icon by keyword from Iconify. |
| [`web-picker/SKILL.md`](web-picker/SKILL.md) | `pip install web-picker` | Pick between 2-9 rendered HTML variants. |
| [`audio-picker/SKILL.md`](audio-picker/SKILL.md) | `pip install audio-picker` *(coming soon)* | Pick a sound effect or audio clip by ear. |

Together they cover the "small, judgment-heavy decision" family — visual, audio, and HTML respectively.

## Repository structure

```text
human-picker-skill/
├── README.md
├── README_zh.md
├── svg-picker/
│   └── SKILL.md
├── audio-picker/
│   └── SKILL.md
└── web-picker/
│   └── SKILL.md
```

Each skill folder is self-contained: it has exactly one `SKILL.md` with the YAML frontmatter the agent runtime needs. There is no umbrella skill — the agent runtime auto-matches each skill's `description` field against the user's task.

## Install

Clone this repository, then symlink the skills you want into your agent runtime's skills directory.

For Claude Code and Agent Skills compatible runtimes, common locations include:

```text
.claude/skills/
~/.claude/skills/
.agents/skills/
~/.agents/skills/
```

Example: install all three skills:

```bash
git clone https://github.com/human-picker/human-picker-skill.git /tmp/hp
ln -s /tmp/hp/svg-picker    ~/.claude/skills/svg-picker
ln -s /tmp/hp/audio-picker  ~/.claude/skills/audio-picker
ln -s /tmp/hp/web-picker    ~/.claude/skills/web-picker
```

Or install only one (e.g. just `svg-picker`):

```bash
ln -s /tmp/hp/svg-picker ~/.claude/skills/svg-picker
```

After symlinking, reload or restart your agent runtime so it discovers the new skills.

## Picker tool installation

Each skill references a separate Python CLI package installed with `pip`. The skills describe the tool's CLI contract; the tools themselves live in their own repositories:

```bash
pip install svg-picker
pip install web-picker
# pip install audio-picker   # once released
```

Install only the picker tools whose skills you enabled.

## How the AI uses these skills

Each `SKILL.md` contains:

- A `description` field in YAML frontmatter — your agent runtime uses this to decide when to auto-load the skill.
- A usage section explaining the CLI invocation and what stdout / stderr look like.
- A cancellation policy telling the AI not to silently fall back to a default when the human closes the picker window without confirming.

The AI matches the user's task against each skill's description and loads the relevant skill(s) on demand. You do not need to instruct the agent to load a specific skill — the runtime does that automatically.
