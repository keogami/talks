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
- `presentation-outline.md` - Speaker notes and outline
- Presentation markdown files (to be created from outlines)
