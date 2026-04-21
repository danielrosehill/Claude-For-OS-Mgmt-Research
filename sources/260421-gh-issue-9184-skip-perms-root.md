---
url: https://github.com/anthropics/claude-code/issues/9184
title: "[BUG] --dangerously-skip-permissions cannot be used with root/sudo privileges for security reasons"
type: community
captured: 2026-04-21
tags: [dangerously-skip-permissions, root, sudo, regression, github-issue]
---

Bug report (closed) filed on Claude Code 2.0.10: starting with the flag under root or sudo now
errors out with "cannot be used with root/sudo privileges for security reasons." Filed as a
regression — previous versions permitted it. The reporter's argument is the core sysadmin tension
in miniature: users running in a sandbox (root container, VM, bastion) should be able to make
their own trust decision; the paternalistic block forces migration to workarounds or to competing
tools. Direct evidence that Anthropic has been *narrowing* the sledgehammer flag, not loosening
it, and that admin workflows are increasingly collateral.
