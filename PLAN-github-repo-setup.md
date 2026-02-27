# Plan: Create Public GitHub Repo for Medical AI Safety Papers

**Status**: Completed
**Tool**: Claude Code
**Created**: 2026-02-27
**Last Updated**: 2026-02-27

## Overview
Create a clean, public-facing GitHub repository (`tekram/medical-ai-safety`) that publishes the three red-teaming papers in well-structured, readable markdown format with figures, a central index, and an attack taxonomy reference.

## Tasks
- [x] Fix remaining stale text in paper1_draft.md (harmful compliance rates, duplicate limitations)
- [x] Fix stale refusal rate numbers in paper1 (Gemini 3.1 Pro was 71.2%, final is 42.5%)
- [x] Create repo directory structure: papers/paper0-original, paper1-frontier, paper2-longitudinal
- [x] Copy all three paper markdown files and figures
- [x] Add inline figure image embeds to paper1 and paper2 markdown
- [x] Write per-paper README.md files (summary, key findings, figure previews)
- [x] Write main README.md (index, cross-cutting findings, methodology, repo structure)
- [x] Write data/attack_taxonomy.md (full 24-row taxonomy reference)
- [x] Create GitHub repo (public): tekram/medical-ai-safety
- [x] Initial commit and push

## Files Created
- `README.md` — Main landing page / index
- `papers/paper0-original/README.md` — Paper 0 summary
- `papers/paper0-original/paper.md` — Full paper text
- `papers/paper0-original/figures/` — 3 figures
- `papers/paper1-frontier/README.md` — Paper 1 summary + result table
- `papers/paper1-frontier/paper.md` — Full paper text with inline figures
- `papers/paper1-frontier/figures/` — 4 figures
- `papers/paper2-longitudinal/README.md` — Paper 2 summary + result tables
- `papers/paper2-longitudinal/paper.md` — Full paper text with inline figures
- `papers/paper2-longitudinal/figures/` — 3 figures
- `data/attack_taxonomy.md` — Full 8-category, 24-sub-strategy taxonomy

## Completion Summary
**Completed on**: 2026-02-27
**Completed by**: Claude Code

All three papers published to public GitHub repo `tekram/medical-ai-safety`. Main README serves as index with result tables for all papers. Each paper has its own README with summary and figures, plus the full paper.md. Attack taxonomy published as reference document. All figures embedded inline in the paper markdowns for readable rendering on GitHub.
