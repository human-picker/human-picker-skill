---
name: human-picker
description: Use web-picker and svg-picker to ask a human to visually choose between small, reversible UI, HTML, or icon options during AI-assisted development.
license: MIT
compatibility:
  tools:
    - web-picker
    - svg-picker
---

# human-picker

Use human visual judgment for small, reversible design decisions during AI-assisted development.

## Purpose

`human-picker` is a skill for deciding when and how to ask a human to visually choose between generated options instead of forcing the AI to guess.

It coordinates two tools:

- `web-picker`: compare rendered HTML variants and return the selected file path. See [`references/web-picker.md`](references/web-picker.md).
- `svg-picker`: search Iconify icons and return selected SVG source. See [`references/svg-picker.md`](references/svg-picker.md).

Chinese reading version: [`SKILL_zh.md`](SKILL_zh.md).

## When to Use

Use this skill when:

- The decision is visual or preference-based.
- Several plausible UI, layout, color, or icon options exist.
- A human can decide faster by looking than by reading descriptions.
- The choice is small, reversible, and does not require a full review process.

Typical cases:

- Choosing between landing page hero variants.
- Choosing a card, button, navigation, or empty-state visual style.
- Choosing an icon for an action, feature, or state.
- Comparing generated HTML mockups before implementing the selected direction.

## When Not to Use

Do not use this skill for:

- Architecture decisions.
- Non-visual implementation choices.
- Security, correctness, or performance review.
- Large open-ended design work.
- More than 9 HTML candidates.
- Situations where the user requested fully autonomous execution.

## Tool Selection

- Use `web-picker` for HTML/UI visual variants. Read [`references/web-picker.md`](references/web-picker.md) before calling it.
- Use `svg-picker` for icon selection. Read [`references/svg-picker.md`](references/svg-picker.md) before calling it.

## Cancellation Policy

Cancellation is meaningful human feedback.

If the user closes a picker without confirming:

- Stop the visual-selection flow.
- Report that the selection was cancelled.
- Ask whether to retry, reduce options, proceed manually, or choose a default.
- Do not silently pick a fallback.

## Output Handling

After selection:

- Record which option was selected.
- Continue implementation using the selected file or SVG.
- Preserve the user’s choice unless they explicitly ask to change it.
