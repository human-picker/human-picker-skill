---
name: svg-picker
description: Pick an icon visually from search results when the AI needs to choose a UI icon. Search Iconify by keyword, present a GUI window with multiple icon candidates, the human selects one or more, SVG source returns via stdout. Use for action/feature/status/nav icons in UI; not for complex illustrations or brand-locked graphics.
license: MIT
---

# svg-picker

Visual icon selection for AI coding agents.

## When to Use

Pick this skill when:

- The task involves choosing an icon for a UI action, feature, status, or navigation element.
- The user said "icon" or similar and visual judgment is faster than description.
- Several plausible icons exist and the choice is small, reversible, and preference-based.

Do not use for:

- Non-icon complex illustrations.
- Brand-locked graphics with strict license requirements.
- Cases where the user already specified a concrete icon library and icon name.

## Install

```bash
pip install svg-picker
```

No API keys or other configuration required.

## Usage

```bash
svg-picker <keyword> [<keyword> ...] [--theme cream|sky|dark] [--per-page N]
```

- **One or more keywords** — pass 2-4 near-synonyms when possible (`svg-picker home house dwelling`). The human can switch between them in-window via the search dropdown without you having to relaunch — saves a turn when the first keyword returns poor results.
- **`--theme cream|sky|dark`** — background theme; also live-cycleable via the 🎨 button in the window.
- **`--per-page N`** — icons per page (default 10).

The window also supports adding new keywords on the fly via the input field at the bottom of the search dropdown.

## Output

After the human confirms, **stdout** contains selected SVG source, one block per selection:

```html
<!-- mdi:home -->
<svg ...>...</svg>

```

Multiple selections are emitted one after another, each preceded by its `<!-- iconify_id -->` comment.

If the human closes the window without confirming, **stderr** contains one of:

```
[svg-picker] cancelled: window closed without selection
[svg-picker] cancelled: window closed with N icons selected but not confirmed (id1, id2, ...)
```

Read stderr to distinguish cancellation from program crash.

## Workflow

1. Pick 1-4 near-synonym keywords describing the icon.
2. Run `svg-picker <keywords...>` via subprocess; capture stdout and stderr.
3. Wait for the human to select and confirm in the window.
4. Parse stdout for SVG source blocks.
5. Embed in code or save as an asset.

## Cancellation Handling

Cancellation is meaningful human feedback. If you see `[svg-picker] cancelled` in stderr:

- Do not silently pick a fallback icon.
- Tell the user the icon selection was cancelled.
- Ask whether to retry with different keywords, manually specify an icon, or use a default.
