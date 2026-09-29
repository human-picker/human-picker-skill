# human-picker skill

Agent skill for asking humans to make small, reversible visual choices during AI-assisted development.

中文文档: [README_zh.md](README_zh.md)

`human-picker` is a human-in-the-loop visual decision skill. It helps AI coding agents know when to stop guessing and ask a human to choose between visual options.

## What it does

This skill guides an AI agent to:

- identify small visual decision points;
- generate or prepare clear candidates;
- call an appropriate picker tool;
- respect human cancellation;
- continue implementation based on the selected result.

Current picker references include:

- `web-picker` — compare rendered HTML/UI variants.
- `svg-picker` — choose SVG icons visually.

The skill is intentionally extensible. Future picker tools can be added as additional reference documents without changing the core idea.

## Repository structure

```text
human-picker-skill/
├── README.md
├── README_zh.md
├── SKILL.md
└── references/
    ├── web-picker.md
    └── svg-picker.md
```

`SKILL.md` is the actual skill entry point. The files under `references/` contain tool-specific usage notes.

## Install

Copy or clone this repository into a skills directory supported by your agent runtime.

For Pi / Agent Skills compatible runtimes, common locations include:

```text
.agents/skills/human-picker/
~/.agents/skills/human-picker/
```

Example:

```bash
git clone https://github.com/human-picker/human-picker-skill.git .agents/skills/human-picker
```

Then reload or restart your agent runtime so it can discover the skill.

## Usage

When the task involves a small visual choice, invoke or allow the agent to load the `human-picker` skill.

Examples:

- choose between several landing page hero mockups;
- pick a button or card visual style;
- select an icon for an action;
- compare generated HTML options before implementing the selected direction.

## Picker tool installation

This repository contains the skill instructions only. The actual picker tools are Python CLI packages installed with `pip`, not Node/npm packages.

```bash
pip install web-picker
pip install svg-picker
```

Install only the picker tools you need. Future picker integrations may have their own installation methods.
