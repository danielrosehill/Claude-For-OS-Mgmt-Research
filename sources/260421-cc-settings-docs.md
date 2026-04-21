---
url: https://code.claude.com/docs/en/settings
title: "Claude Code settings — Claude Code Docs"
type: docs
captured: 2026-04-21
tags: [settings.json, permissions, additionalDirectories, sandbox, managed-settings, hierarchy]
---

Canonical reference for `settings.json`. Documents the five-level precedence (managed > CLI args >
`.claude/settings.local.json` > `.claude/settings.json` > `~/.claude/settings.json`) and the
`permissions` block: `allow` / `deny` / `ask` / `additionalDirectories` / `defaultMode`. Array
settings merge across scopes rather than replace. Crucial for the sysadmin framing:
`additionalDirectories` is the declarative lever for widening the de-facto sandbox beyond the
launch cwd, and `sandbox.filesystem.{allowWrite,denyWrite,allowRead,denyRead}` plus
`sandbox.network.{allowedDomains,deniedDomains}` expose fine-grained Linux sandbox controls.
Managed settings can pin `disableBypassPermissionsMode` / `disableAutoMode`, which is where an
admin-shaped policy would live.
