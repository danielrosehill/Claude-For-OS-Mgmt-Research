# Research Workspace — Deep Research

**Variant:** `deep-research`
**Name:** Claude-For-OS-Mgmt-Research
**Created:** 2026-04-21
**Owner:** Daniel Rosehill (Jerusalem, Israel — IST/IDT, Ubuntu 25.10)
**Central question:** <RESEARCH_QUESTION — fill in: the use of Claude / Claude Code for operating-system management workflows>

## Purpose

This is a general-purpose deep-research workspace. Your job is to investigate the central question thoroughly, log every source, synthesise what they say, and produce structured outputs.

## Startup

On each new session, run `/research-space:research-init` first to load context, sources index, and prior outputs.

## Workflow

1. Capture sources with `/research-space:source-log <url-or-ref>` as you find them.
2. Once you have a cluster of sources, run `/research-space:summarize-sources` to produce per-source summaries and a rolled-up synthesis.
3. For focused questions, run `/research-space:deep-dive <topic>` — it drives an end-to-end sub-investigation and writes a structured report to `outputs/`.
4. Export a final report with `/research-space:export-report` when ready.

## Conventions

- **Sources**: `sources/YYMMDD-<slug>.md` with YAML front-matter (url, title, type, captured, tags).
- **Outputs**: `outputs/<kind>-<slug>-YYMMDD.md`. Maintain `outputs/INDEX.md`.
- **Context**: long-lived framing notes live in `context/`. These feed every deep-dive.
- **Notes**: scratch thinking lives in `notes/` and is not part of the final report.

## Rules

- Every non-obvious claim in outputs must cite at least one source file.
- Distinguish primary sources from secondary commentary in front-matter `type`.
- Flag uncertainty plainly — mark speculative claims with `(speculation)` inline.
