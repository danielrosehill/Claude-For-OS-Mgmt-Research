---
url: https://www.ricardodecal.com/guides/running-sudo-commands-in-claude-code-and-cursor/
title: "Running sudo commands in Claude Code and Cursor"
type: secondary
captured: 2026-04-21
tags: [sudo, SUDO_ASKPASS, non-interactive, linux]
---

Short guide to the `SUDO_ASKPASS` pattern for letting Claude Code run `sudo` non-interactively on
Linux: Claude Code can't drive an interactive password prompt, so you provide a non-interactive
askpass helper and invoke `sudo -A`. Useful as the minimal, practical mitigation for the sudo
pain point on a workstation, and as a reference point when comparing to Issue #1135's more
security-conscious variant.
