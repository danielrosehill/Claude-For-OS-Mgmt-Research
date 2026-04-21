---
url: https://github.com/anthropics/claude-code/issues/3490
title: "[BUG] Perhaps another flag for --dangerously-skip-permissions that is OK with root"
type: community
captured: 2026-04-21
tags: [dangerously-skip-permissions, root, docker, feature-request, closed-not-planned]
---

Feature request (ezyang, July 2025) asking for a sibling flag that permits bypass mode under root
in sandboxed environments like Docker containers. **Closed as not planned.** Useful companion to
#9184: it shows an explicit Anthropic product decision *not* to provide an escape hatch for
root-in-container, which is one of the canonical sysadmin deployment shapes. Establishes that the
upstream position is "configure your container to not run as root" rather than "we'll give you a
supported opt-in."
