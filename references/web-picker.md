# web-picker 简要使用说明

`web-picker` 用于让人类在多个 HTML 视觉方案中选择一个。

返回主文档：[`../SKILL.md`](../SKILL.md) / 中文说明：[`../SKILL_zh.md`](../SKILL_zh.md)

相关工具文档：[`svg-picker.md`](./svg-picker.md)

## 适用场景

适合比较：

- 首页首屏方案
- 卡片、按钮、导航栏、空状态页面样式
- 颜色、排版、布局方向
- 任何可以写成完整 HTML 文件的视觉候选方案

不适合：

- 架构决策
- 代码正确性审查
- 非视觉类选择
- 超过 9 个候选方案

## 安装

`web-picker` 是 Python CLI 工具，通过 `pip` 安装，不是 Node/npm 包。

```bash
pip install web-picker
```

开发模式安装：

```bash
pip install -e /path/to/web-picker
```

如果遇到 `QtWebEngineWidgets is not available in this install`，通常需要补装：

```bash
pip install PySide6-Addons
```

## 基本命令

```bash
web-picker option-a.html option-b.html option-c.html
```

候选数量：1-9 个 HTML 文件。

常用参数：

```bash
web-picker a.html b.html --theme dark
web-picker a.html b.html --maximize
web-picker a.html b.html --width 1600 --height 900
```

主题可选：

- `cream`
- `sky`
- `dark`

## AI Agent 使用流程

1. 生成 2-9 个完整、独立的 HTML 文件。
2. 文件名要能表达方案差异，例如：
   - `hero-centered.html`
   - `hero-split-image.html`
   - `hero-gradient.html`
3. 运行 `web-picker`。
4. 读取 stdout。
5. 根据用户选择继续实现。

## 输入要求

每个候选都应该是完整 HTML 文档：

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Option A</title>
</head>
<body>
  ...
</body>
</html>
```

建议：

- 尽量自包含。
- 使用真实或接近真实的文案。
- 候选之间要有明显视觉差异。
- 优先提供 2-4 个强方案，不要堆很多弱方案。

## 输出处理

用户确认后，`web-picker` 会把选中文件的绝对路径输出到 stdout：

```text
/path/to/selected-option.html
```

AI 应该使用这个文件作为后续实现依据。

## 取消处理

如果用户关闭窗口或按 Esc，stderr 会包含：

```text
[web-picker] cancelled: ...
```

处理原则：

- 不要自动替用户选择备用方案。
- 告诉用户选择已取消。
- 询问是否重试、减少候选、手动指定，或允许 AI 使用默认方案。
