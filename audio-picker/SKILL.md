---
name: audio-picker
description: Pick a sound effect or audio clip visually by ear when the AI needs to choose audio for UI feedback, transitions, or content. Search Freesound by keyword, present a GUI window with audio candidates and inline preview playback, the human previews and picks, the selected sound preview URL returns via stdout. Use for UI sound effects, transition stings, ambient loops, and any choice where ears are faster than descriptions.
license: MIT
---

# audio-picker

> **Status**: skill published ahead of CLI release. The `audio-picker` package is in active development (see the `audio-picker/` directory in the same repository family). This file documents the planned interface; concrete CLI examples will be added when v0.2 ships.

Visual sound selection for AI coding agents. Pairs with `svg-picker` (visuals) and `web-picker` (layouts) to cover the "small, reversible, judgment-heavy decision" family — visual, audio, and HTML respectively.

## When to Use

Pick this skill when:

- The task involves choosing a sound effect, UI feedback tone, transition sting, music loop, or any audio asset.
- Audio needs to be heard rather than described — ear judgment is faster than text description.
- Multiple plausible audio options exist and the choice is small, reversible, and preference-based.

Do not use for:

- Audio where the user already specified an exact file path or asset name.
- Voice acting / long-form narration (tool is tuned for short effects and loops).
- Cases requiring strict commercial license vetting — note Freesound licenses vary per upload and the caller is responsible for compatibility checks.

## Planned Install

```bash
pip install audio-picker
```

Then create a Freesound API token at <https://freesound.org/apiv2/apply> and put it in `.env`:

```ini
# .env
FREESOUND_API_TOKEN=your_token_here
```

`audio-picker` will auto-create an empty `.env` on first launch if one does not exist.

## Planned Usage

```bash
audio-picker <keyword> [<keyword> ...]
```

Pass near-synonym keywords when possible — the in-window search dropdown supports switching and adding keywords without a relaunch, matching the `svg-picker` UX.

## Planned Output

After the human confirms, **stdout** contains selected sound references, one block per selection:

```text
<!-- freesound:123456 -->
https://cdn.freesound.org/previews/123456/123456_1648-lq.mp3

```

Each block is a `<!-- freesound:<id> -->` comment followed by a preview URL. The caller can download from the URL or look up metadata via Freesound.

If the human closes the window without confirming, **stderr** contains one of:

```
[audio-picker] cancelled: window closed without selection
[audio-picker] cancelled: window closed with N sounds selected but not confirmed (...)
```

## Workflow

1. Pick 1-4 near-synonym keywords describing the audio (e.g. `whoosh zap swoosh` for a transition).
2. Run `audio-picker <keywords...>` via subprocess; capture stdout and stderr.
3. Wait for the human to preview clips in the window and confirm.
4. Parse stdout for sound IDs and preview URLs.
5. Download the chosen audio, embed in code, or save as an asset.

## Cancellation Handling

Same policy as `svg-picker` and `web-picker`: cancellation is meaningful human feedback. If you see `[audio-picker] cancelled` in stderr, do not silently fall back to a default sound — ask the user how to proceed.

## See Also

- [`../svg-picker/SKILL.md`](../svg-picker/SKILL.md) — visual icon selection
- [`../web-picker/SKILL.md`](../web-picker/SKILL.md) — visual HTML variant selection
