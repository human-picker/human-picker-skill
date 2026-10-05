---
name: web-picker
description: Pick between 2-9 rendered HTML/UI variants visually when the AI needs to choose a layout, hero, component style, or color scheme. Pass candidate HTML files, present a GUI window with side-by-side previews, the human highlights one then confirms, the selected file path returns via stdout. Use for hero variants, card/button styles, navigation layouts, empty-state visuals, and any HTML-renderable design decision.
license: MIT
---

# web-picker

Visual HTML variant selection for AI coding agents.

## When to Use

Pick this skill when:

- The AI has 2-9 candidate HTML files to choose from — hero variants, page layouts, component styles, color schemes.
- Visual judgment is faster than reading markdown descriptions of each option.
- The decision is small, reversible, and bounded to 1-9 candidates.

Do not use for:

- Architecture decisions.
- Non-visual implementation choices.
- More than 9 candidates.
- Situations where the user requested fully autonomous execution.

## Install

```bash
pip install web-picker
```

If you see `QtWebEngineWidgets is not available in this install`, run `pip install PySide6-Addons` — some minimal PySide6 installs ship only the Essentials subset without the WebEngine.

On Windows + Microsoft Store Python, you may hit long-path errors while installing PySide6; see <https://github.com/human-picker/web-picker#install> for the workaround (short venv path, Long Path support).

## Usage

```bash
web-picker <file1.html> [file2.html ...]    # 2-9 files
```

Optional flags:

- `-t`, `--theme cream|sky|dark` — background theme (default `cream`).
- `--width <px>`, `--height <px>` — window size (default: auto-fit screen).
- `--maximize` — open maximized (cannot combine with `--width`/`--height`).
- `--slider-handle <px>` — zoom-slider knob width; recommended `14`-`24` for trackpad.

## Two-Step Pick

`web-picker` uses a **two-step pick** to reduce mistakes:

1. **Highlight** — click a card (or press `1`-`9`) — the preview is outlined as the candidate.
2. **Confirm** — click **Confirm** (or press `Enter`) — the highlighted option is committed.

This lets hesitant humans change their mind before committing. Closing the window at any point cancels.

## Output

After the human confirms, **stdout** contains the absolute path of the selected HTML file.

```text
C:\path\to\selected.html
```

If the human closes the window without confirming, **stderr** contains:

```
[web-picker] cancelled: ...
```

Read stderr to distinguish cancellation from program crash.

## Workflow

1. Write 2-9 candidate HTML files to disk (use whatever filenames you like — paths are passed verbatim to `web-picker`).
2. Run `web-picker <files...>` via subprocess; capture stdout and stderr.
3. Wait for the human to highlight + confirm in the window.
4. Read the file path from stdout.
5. Continue implementation using the selected file.

## Keyboard Shortcuts

- `1`-`9` — highlight option N
- `Enter` — confirm highlighted option
- `Esc` — cancel (close window)

## Cancellation Handling

Cancellation is meaningful human feedback. If you see `[web-picker] cancelled` in stderr:

- Do not silently pick a fallback HTML.
- Tell the user the selection was cancelled.
- Ask whether to retry with reduced candidates, proceed manually, or skip the visual comparison.
