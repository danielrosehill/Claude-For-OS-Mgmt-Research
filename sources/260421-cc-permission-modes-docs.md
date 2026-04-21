---
url: https://code.claude.com/docs/en/permission-modes
title: "Choose a permission mode — Claude Code Docs"
type: docs
captured: 2026-04-21
tags: [permissions, modes, bypassPermissions, auto, dontAsk, acceptEdits, plan, protected-paths]
---

Canonical documentation of Claude Code's permission modes: `default` (reads only), `acceptEdits`
(adds file edits + a fixed list of filesystem Bash commands inside the working directory), `plan`,
`auto` (classifier-gated), `dontAsk` (CI-style pre-approved only), and `bypassPermissions`
(equivalent to `--dangerously-skip-permissions`). Establishes the central constraint for sysadmin
use: auto-approval in `acceptEdits` is scoped to the working directory or `additionalDirectories`;
everything outside still prompts. `bypassPermissions` can only be entered by *starting* the session
with an enabling flag, and can be globally disabled via managed settings. Lists the "protected
paths" (`.git`, `.claude`, `.bashrc`, `.mcp.json`, etc.) that are never auto-approved in any mode.
Auto mode requires v2.1.83+, paid plan, specific models, Anthropic API only — not a general
replacement for bypass.
