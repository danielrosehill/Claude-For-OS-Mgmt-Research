---
url: https://code.claude.com/docs/en/hooks
title: "Hooks reference — Claude Code Docs"
type: docs
captured: 2026-04-21
tags: [hooks, PreToolUse, permissionDecision, PermissionRequest, interception]
---

Reference for Claude Code hooks — the deterministic control layer that runs outside the model.
The load-bearing events for a sysadmin profile are `PreToolUse` (can emit
`permissionDecision: allow | deny | ask | defer` and even rewrite `updatedInput`) and
`PermissionRequest` (can return `behavior: allow|deny` plus `updatedPermissions`, i.e. persist a
rule). Hooks can be command, HTTP, prompt, or agent; they can target MCP tools via
`mcp__<server>__<tool>` matchers. Relevant for building a user-owned authorisation layer that
pre-approves sysadmin-shaped calls (sudo, /etc writes, systemctl) without relying on
`bypassPermissions`. Writes to protected paths route through hooks in every mode except `dontAsk`.
