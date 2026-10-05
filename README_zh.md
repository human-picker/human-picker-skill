# human-picker skills

Claude Code / Agent Skills 合集 —— 在 AI 辅助开发中，让人类对小型、可逆的视觉与音频决策做选择。

英文文档：[README.md](README.md)

仓库里每个 skill 对应一个 picker 工具。它们互相独立，只安装你需要的就行。

## 包含的 skill

| Skill | CLI | 适用场景 |
|---|---|---|
| [`svg-picker/SKILL.md`](svg-picker/SKILL.md) | `pip install svg-picker` | 通过关键词从 Iconify 选图标 |
| [`web-picker/SKILL.md`](web-picker/SKILL.md) | `pip install web-picker` | 在 2-9 个 HTML 渲染方案中选一个 |
| [`audio-picker/SKILL.md`](audio-picker/SKILL.md) | `pip install audio-picker`（待发布） | 用耳朵挑音效或音频素材 |

三者一起覆盖"小型、判断密集"决策的全家桶 —— 视觉、音频、HTML 分别对应。

## 仓库结构

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

每个 skill 目录都是自包含的：只有一个 `SKILL.md`，里面有 agent runtime 需要的 YAML frontmatter。**没有 umbrella skill** —— agent runtime 会根据每个 skill 自己的 `description` 字段自动匹配用户的任务。

## 安装

克隆这个仓库，然后把你想要的 skill 软链到 agent runtime 的 skills 目录。

Claude Code 以及兼容 Agent Skills 协议的 runtime，常见位置：

```text
.claude/skills/
~/.claude/skills/
.agents/skills/
~/.agents/skills/
```

示例：三个 skill 全部安装：

```bash
git clone https://github.com/human-picker/human-picker-skill.git /tmp/hp
ln -s /tmp/hp/svg-picker    ~/.claude/skills/svg-picker
ln -s /tmp/hp/audio-picker  ~/.claude/skills/audio-picker
ln -s /tmp/hp/web-picker    ~/.claude/skills/web-picker
```

只要一个（比如只要 `svg-picker`）：

```bash
ln -s /tmp/hp/svg-picker ~/.claude/skills/svg-picker
```

软链完之后，重启或重载 agent runtime，让它重新扫描 skills 目录。

## Picker 工具的安装

每个 skill 都引用一个独立的 Python CLI 包，通过 `pip` 安装。Skill 文件只描述 CLI 契约，真正的工具代码在各自的仓库：

```bash
pip install svg-picker
pip install web-picker
# pip install audio-picker   # 等发布
```

只装你启用了 skill 的那个工具。

## AI 怎么用这些 skill

每个 `SKILL.md` 包含：

- YAML frontmatter 里的 `description` 字段 —— agent runtime 用它判断什么时候自动加载这个 skill。
- Usage 段 —— 讲 CLI 怎么调，stdout / stderr 长什么样。
- 取消处理原则 —— 告诉 AI 在人类没点 Confirm 直接关窗时**不要**静默选用默认值。

AI 会拿用户的任务去匹配每个 skill 的 description，按需加载。你不需要手动告诉 agent 加载哪个 skill，runtime 自己会判断。
