---
url: https://github.com/anthropics/claude-code/issues/36168
title: "[BUG] Bypass/dangerously skip permissions now broken in all Claude Code versions newer than v2.1.77"
type: community
captured: 2026-04-21
tags: [dangerously-skip-permissions, regression, v2.1.77, v2.1.79, bypassPermissions]
---

User-reported regression: on versions >= 2.1.79, `bypassPermissions` still prompts for every file
edit and script execution despite being correctly configured; rolling back to 2.1.77 restores the
intended behaviour. Pins a specific version boundary at which the sledgehammer stopped being
reliable on at least some platforms (VS Code, macOS). Directly illustrates the workspace's thesis
that "sandbox defaults tighten every release and the sysadmin workflow regresses in parallel" —
this time the behaviour of `bypassPermissions` itself silently narrowed.
