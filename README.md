# human-picker skill

Agent skill for asking humans to make small, reversible visual choices during AI-assisted development.

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

## Notes

This repository contains the skill instructions only. The actual picker tools should be installed separately when needed.

---

# human-picker skill（中文）

用于在 AI 辅助开发过程中，让人类对小型、可逆的视觉决策进行选择的 Agent skill。

`human-picker` 是一个 human-in-the-loop 视觉决策 skill。它帮助 AI 编程 Agent 判断：什么时候不应该继续猜，而应该让人类从视觉方案中选择。

## 它做什么

这个 skill 会指导 AI Agent：

- 识别小型视觉决策点；
- 生成或准备清晰的候选方案；
- 调用合适的 picker 工具；
- 尊重用户取消；
- 根据用户选择继续后续实现。

当前包含的 picker 参考文档：

- `web-picker`：用于比较 HTML/UI 渲染方案。
- `svg-picker`：用于可视化选择 SVG 图标。

这个 skill 是可扩展的。未来可以继续添加更多 picker 工具，只需要增加新的参考文档，不需要改变核心思想。

## 仓库结构

```text
human-picker-skill/
├── README.md
├── SKILL.md
└── references/
    ├── web-picker.md
    └── svg-picker.md
```

`SKILL.md` 是真正的 skill 入口文件。`references/` 下的文件是具体工具的使用说明。

## 安装

把这个仓库复制或克隆到你的 Agent runtime 支持的 skills 目录中。

对于 Pi / Agent Skills 兼容运行时，常见位置包括：

```text
.agents/skills/human-picker/
~/.agents/skills/human-picker/
```

示例：

```bash
git clone https://github.com/human-picker/human-picker-skill.git .agents/skills/human-picker
```

然后重载或重启你的 Agent runtime，让它重新发现 skill。

## 使用场景

当任务中出现小型视觉选择时，可以让 Agent 加载或使用 `human-picker` skill。

例如：

- 在多个 landing page 首屏 mockup 中选择；
- 选择按钮、卡片等 UI 视觉样式；
- 为某个操作选择图标；
- 在正式实现前比较多个 HTML 方案。

## 注意

这个仓库只包含 skill 指令本身。实际的 picker 工具需要按需另行安装。
