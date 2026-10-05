# svg-picker 简要使用说明

`svg-picker` 用于通过关键词搜索 Iconify 图标，并让人类从视觉结果中选择 SVG。

返回主文档：[`../SKILL.md`](../SKILL.md) / 中文说明：[`../SKILL_zh.md`](../SKILL_zh.md)

相关工具文档：[`web-picker.md`](./web-picker.md)

## 适用场景

适合选择：

- 功能图标
- 操作按钮图标
- 状态图标
- 导航图标
- 空状态插画中的简单 SVG 图标

不适合：

- 非图标类复杂插画
- 需要品牌授权或商标准确性的图形
- 用户已经明确指定具体图标库和图标名的情况

## 安装

`svg-picker` 是 Python CLI 工具，通过 `pip` 安装，不是 Node/npm 包。

```bash
pip install svg-picker
```

开发模式安装：

```bash
pip install -e /path/to/svg-picker
```

## 基本命令

```bash
svg-picker <关键词> [<关键词> ...] [--theme cream|sky|dark] [--per-page N]
```

示例：

```bash
svg-picker home
svg-picker home --theme sky
svg-picker search -t dark
svg-picker settings --per-page 20

# 一次传多个近义关键词，用户可以在窗口内切换：
svg-picker home house dwelling
svg-picker sword blade katana claymore
```

常用参数：

- `--theme cream|sky|dark`：选择界面主题。窗口内点击 🎨 按钮可实时循环切换，无需重启。
- `--per-page N`：每页显示多少个图标。

## 多关键词与窗口内切换

svg-picker 1.1+ 支持一次传入多个关键词，标题栏出现一个 `Search: <当前关键词> ▾` 下拉按钮：

- **切换**：点击下拉中的关键词按钮即可查询新的；当前页、选中、缓存都会重置。
- **临时新增**：在下拉底部的输入框里敲新关键词 + `Enter`（或点 **+**），立刻把它加进列表并查询。

这样如果第一个关键词搜出来的图标不好用，用户不用关窗、不用让你重跑，AI 也不会白白浪费一轮 reject。

## AI Agent 使用流程

1. 根据语义选择一个（或一组近义）英文关键词。
2. 运行 `svg-picker <关键词...>`。
3. 等待用户选择并确认。
4. 从 stdout 读取 SVG 源码。
5. 将 SVG 嵌入代码或保存为资源文件。

## 关键词建议

优先使用简短、常见的英文 UI 词：

- `home`
- `search`
- `settings`
- `user`
- `menu`
- `close`
- `arrow`
- `edit`
- `delete`
- `download`
- `upload`

如果结果不好，可以换同义词：

- `trash` / `delete`
- `cog` / `settings`
- `person` / `user`
- `magnify` / `search`

## 输出处理

用户确认后，`svg-picker` 会把 SVG 源码输出到 stdout。

输出可能包含 Iconify 注释：

```html
<!-- mdi:home -->
<svg ...>...</svg>
```

AI 可以：

- 保留注释，方便追踪图标来源。
- 只提取 `<svg>...</svg>` 部分嵌入项目。

## 取消处理

如果用户关闭窗口，stderr 会包含：

```text
[svg-picker] cancelled: ...
```

处理原则：

- 不要随机选择一个图标。
- 不要静默换成默认图标。
- 告诉用户图标选择已取消。
- 询问是否换关键词重试、手动指定图标，或允许 AI 使用默认图标。
