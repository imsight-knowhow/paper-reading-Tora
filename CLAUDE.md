# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a paper reading notes repository for the CVPR 2025 paper "Tora: Trajectory-oriented Diffusion Transformer for Video Generation". The repository contains:
- LaTeX source files for the CVPR paper (in `papers/tora/`)
- Slidev presentation setup for creating slides about the paper (in `slides/`)
- Structured context directories following the Context Directory Guide pattern (in `context/`)

## Common Commands

### Slides Development
- **Start slides dev server**: `npm run slides:dev` - Opens interactive slidev server for presentation development
- **Build slides**: `npm run slides:build` - Builds static slides output
- **Export slides**: `npm run slides:export` - Exports slides to PDF/PNG format

### LaTeX Paper Compilation
The LaTeX paper is located in `papers/tora/`. To compile:
- Main file: `papers/tora/main.tex`
- Uses CVPR 2025 template with `cvpr.sty`
- Bibliography: `main.bib` with `ieeenat_fullname.bst` style

## Architecture & Structure

### Context Directory Pattern
The project follows a structured context management approach with specialized directories:
- `context/design/` - Design documents and architectural decisions
- `context/instructions/` - Development guidelines and templates
- `context/refcode/` - Reference code snippets
- `context/tasks/` - Task tracking with subdirectories for features, fixes, refactor, and tests
- `context/summaries/` - Paper summaries and key insights
- `context/logs/` - Development and research logs

### Key Components
1. **Paper Source** (`papers/tora/`): Contains the full LaTeX source with sections split into individual `.tex` files under `sec/` directory
2. **Presentation** (`slides/`): Markdown-based slides using Slidev framework
3. **Resources** (`slides/res/`): Supporting materials for presentations

## Development Notes

- This is a research-focused repository for understanding and documenting the Tora paper
- No Python/ML code implementation yet - focus is on paper analysis and documentation
- Uses Node.js for slidev presentation tooling
- LaTeX compilation required for paper viewing/editing
- the latex source of the paper `tora-paper` is in `papers/tora`
- you have python, with pixi as env manager
- `slidev` source code is in `context\refcode\slidev`, you can check it when you make error using `slidev`