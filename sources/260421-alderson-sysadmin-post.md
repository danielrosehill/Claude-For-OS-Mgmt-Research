---
url: https://martinalderson.com/posts/how-i-use-claude-code-to-manage-sysadmin-tasks/
title: "How I use Claude Code to manage sysadmin tasks — Martin Alderson"
type: secondary
captured: 2026-04-21
tags: [sysadmin, ssh, bastion, infrastructure-as-markdown, workflow-pattern]
---

Practitioner writeup of a sysadmin workflow on top of Claude Code. Key pattern: per-host task
folders seeded with a `CLAUDE.md` describing the machine, SSH config aliases instead of inline
credentials, ProxyJump through a bastion so Claude never learns the real topology, and
"infrastructure as markdown" docs that regrow context across sessions. Treats SSH / cloud CLI as
the authorisation boundary rather than trying to bend the Claude Code sandbox. Directly relevant
to the remote-admin branch of this research and to the "launch cwd as de-facto scope" workaround.
