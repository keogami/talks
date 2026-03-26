# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains technical talk presentations using **presenterm**, a terminal-based markdown presentation tool.

## Presentation Tool

All presentations use presenterm. See `presenterm-kb.md` for the full syntax and feature reference.

### Running a Presentation

```bash
presenterm path/to/slides.md        # Development mode (hot reload)
presenterm -p path/to/slides.md     # Presentation mode
presenterm -x path/to/slides.md     # Enable code execution
```

## Repository Structure

Each talk lives in its own directory named `{event}-{year}-{month}/` containing:
- `presentation-outline.md` - Speaker notes and outline (created first by the user)
- `presentation.md` - The presenterm slides (created from the outline)
- Image assets (`.png`, `.gif`) used in slides

## Creating a Presentation

1. The user provides a `presentation-outline.md` with the talk structure, flow, and speaker notes
2. Create `presentation.md` from the outline using presenterm syntax from `presenterm-kb.md`
3. Use an existing talk directory (e.g. `rustdelhi-2025-01/`) as a style reference for slide patterns
4. Generate placeholder images with ImageMagick (`magick`) — the user replaces these with real screenshots later

### Style Conventions

- Theme: `catppuccin-latte`
- Title slides: `font_size: 3`, centered, `jump_to_middle`
- Content slides: `font_size: 2`
- Section dividers: `jump_to_middle` + section title + `end_slide`, followed by content slides
- Center content with `column_layout: [1, 5, 1]` or `[1, 3, 1]` using column 1
- Use `speaker_note` comments for presenter context
- Use `<!-- pause -->` for progressive reveals
- Preserve the dry humor and editorial voice from the outline — it's part of the talk style
