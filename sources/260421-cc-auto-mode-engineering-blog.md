---
url: https://www.anthropic.com/engineering/claude-code-auto-mode
title: "Claude Code auto mode: a safer way to skip permissions"
type: primary
captured: 2026-04-21
tags: [auto-mode, classifier, dangerously-skip-permissions, anthropic-framing, approval-fatigue]
---

Anthropic engineering post (Mar 2026) introducing auto mode as the designated replacement for
`--dangerously-skip-permissions` for users who "let Claude work without permission prompts."
Frames the problem as approval fatigue (users approve ~93% of prompts) and positions auto mode as
a classifier-gated middle ground. Critical framing for this research: the post explicitly treats
**actions outside the working directory** as the high-scrutiny category — production deploys,
mass deletes, IAM changes, unrecognised infrastructure are blocked by default. This is the clearest
public statement that Anthropic's mental model of "Claude Code's scope" is *a project directory
plus its repo remotes*; anything else is "external" and must be earned via trusted-infrastructure
config. Directly informs the central thesis that sysadmin use is an out-of-frame use case.
