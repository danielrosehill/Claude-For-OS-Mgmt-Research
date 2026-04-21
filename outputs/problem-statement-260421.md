---
title: Problem statement — Claude Code's cwd-centric sandbox vs OS-management use
kind: problem-statement
captured: 2026-04-21
status: solidified
owner: Daniel Rosehill
sources:
  - sources/260421-cc-auto-mode-engineering-blog.md
  - sources/260421-cc-permission-modes-docs.md
  - sources/260421-cc-settings-docs.md
  - sources/260421-cc-hooks-docs.md
  - sources/260421-cc-changelog.md
  - sources/260421-gh-issue-9184-skip-perms-root.md
  - sources/260421-gh-issue-3490-root-flag-request.md
  - sources/260421-gh-issue-36168-bypass-broken-v2178.md
  - sources/260421-gh-issue-1135-sudo-askpass.md
  - sources/260421-xda-claude-code-mcp-homelab.md
  - sources/260421-jmagar-claude-homelab.md
  - sources/260421-alderson-sysadmin-post.md
  - sources/260421-larcombe-sysadmin-post.md
  - sources/260421-ricardodecal-sudo-guide.md
  - sources/260421-open-interpreter-contrast.md
---

# Problem statement

## Thesis

Claude Code is one of the most effective tools currently available for
operating-system and infrastructure administration by a single authorised
operator working across their own machines (local desktop, LAN hosts, home
server, VPS). Its **default safety model**, however, is built around a
different premise — a developer working inside a project repository — and
encodes that premise deeply enough that sysadmin use is a second-class
citizen of the harness, sustained only by workarounds that are themselves
being progressively narrowed.

The core of the mismatch is not sandboxing in the abstract. It is one
specific design choice: **the current working directory is treated as the
universe of normal operation, and everything outside it is treated as the
high-scrutiny zone.**

## The core blocker — cwd as the unit of trust

Anthropic's published description of Claude Code's auto mode makes this
explicit: the permission classifier's default posture is that reads and
writes *inside* the cwd are routine, and anything *outside* the cwd
(system paths, other users' home directories, remote hosts) is elevated
and gated.[^auto-mode] The documentation for permission modes, settings,
and `additionalDirectories` all reinforce the same axis: scope is defined
relative to a project root.[^modes] [^settings]

For the target use case this framing has several immediate consequences:

1. **Sysadmin work does not have a project root.** The "workspace" for
   debugging a broken audio stack is `/etc`, `~/.config`, `/usr/lib`,
   `journalctl`, and a handful of systemd unit files — none of which sit
   under a repo and none of which form a natural cwd.

2. **Non-code workspaces inherit the same assumption.** Planning
   workspaces, diaries, research workspaces (including this one), and
   media pipelines are directories of state, not codebases, but the
   harness still treats the directory they were launched from as the
   perimeter. Operations that legitimately span `~/` are flagged as
   out-of-scope.

3. **Remote admin dissolves the premise entirely.** Over SSH or via an
   MCP server that runs commands on another host, the concept of a cwd
   on the controlling machine is not a meaningful boundary for what the
   session is actually doing. The harness's sandbox can be strict while
   the session is, in practice, unsandboxed on the remote end.

4. **Every new workspace starts from zero.** Because scope is
   cwd-relative, trust does not accumulate across the set of machines
   and directories the operator legitimately controls. Allowlists are
   per-workspace or per-user, but the mental model is still "this
   project".

This is the number-one blocker. Every downstream workaround in this
research exists because the trust unit is the cwd rather than the
operator.

## The narrowing-workarounds trajectory

Two workarounds are currently in active use:

1. **`--dangerously-skip-permissions`** — the sledgehammer.
2. **Broad `allow` lists in `settings.json`** (per-workspace and user
   level), plus `permissions.defaultMode`, to pre-approve the
   sysadmin-shaped operations that would otherwise prompt on every call.

The trajectory of the first is not stable. Three tracked issues in the
Claude Code repository together show a consistent direction:

- **#9184** — `--dangerously-skip-permissions` was made unusable under
  root / sudo contexts.[^9184]
- **#3490** — an explicit request for a supported equivalent for
  privileged / rootful use was closed not-planned.[^3490]
- **#36168** — after version 2.1.77 the bypass silently changed
  behaviour, breaking configurations that had depended on it.[^36168]

The pattern: the sledgehammer is being quietly tightened rather than
supported. Issue #1135's sudo-askpass discussion reinforces that
privileged ops were not a design target in the first place.[^1135]

Settings-file allowlists are more durable but have their own ceiling:
they expand the set of pre-approved tool invocations, but they do not
change the cwd-centric premise. `additionalDirectories` and
`sandbox.filesystem.allowWrite` extend the declarative filesystem scope,
yet arbitrary Bash retains a separate permission surface that does not
track in lockstep with them. Declarative fs scope and Bash permission
scope are governed by different mechanisms, and closing the gap requires
manual, version-sensitive configuration.

## The accidental escape hatch — MCP

A finding from the sourcing pass worth calling out because the user had
independently arrived at it: **MCP is the de facto route through which
privileged, out-of-cwd operations actually flow** in this usage pattern.
Community writeups document using MCP servers (Home Assistant, Docker,
LAN-device runners, SSH-executing servers) to perform administrative
work that would otherwise be gated by the shell sandbox.[^xda]
[^jmagar]

This is not a published design intention — it is a side effect of MCP
tool calls being classified differently from shell calls, and of MCP
servers being able to execute arbitrary operations on the server side
without the client-side sandbox inspecting them. The implication:

- Much of the practical sysadmin capability the user values rides on an
  architectural seam rather than a first-class feature.
- That seam is useful precisely because it is *not* governed by the
  cwd-centric model — MCP tool scopes are declared by the server and
  granted at install time, not derived from where Claude Code was
  launched.
- A durable solution to the core blocker could plausibly generalise
  the property that makes MCP work for this use case — **operator-granted,
  persistent scope independent of cwd** — rather than trying to widen
  the cwd.

## Structural asymmetry — absence as a finding

The sourcing pass found no first-party Anthropic material framing Claude
Code as a tool for sysadmin or OS administration. Documentation,
engineering posts, and the changelog all position it as developer
tooling.[^auto-mode] [^settings] The community writeups that *do* frame
it as sysadmin tooling are unofficial, fragmented across personal
blogs, and typically include a section on disabling or broadening
safety features to make it work.[^alderson] [^larcombe] [^ricardodecal]

The absence is itself part of the problem. As long as the use case is
not acknowledged as first-class, every release cycle will optimise the
harness for the dev-tool premise, and every tightening of the default
sandbox will produce a parallel regression for this pattern — which is
exactly the lived experience this research is trying to characterise.

## Initial observations carried forward

These are the shaping observations from the sourcing pass, to be tested
and refined in subsequent deep-dives:

1. **cwd as the trust unit is the load-bearing design choice.** Not
   sandboxing per se. Any durable fix has to change that unit, or
   make it configurable per operator rather than per launch.
2. **`--dangerously-skip-permissions` is narrowing, not stabilising.**
   Treat it as a temporary bridge, not a foundation.
3. **MCP is the accidental escape hatch.** Worth a dedicated deep-dive
   on how much of the sysadmin pattern can be moved *onto* MCP
   deliberately, rather than relying on shell-bypass.
4. **Hooks (`PreToolUse`, `PermissionRequest` with `updatedPermissions`)
   are the most under-documented lever** and plausibly the cleanest
   user-side path to a persistent "authorised operator" profile
   without waiting on upstream.
5. **`additionalDirectories` + `sandbox.filesystem.allowWrite` vs Bash
   permissions is a structural gap.** Declarative fs scope does not
   cover arbitrary shell, so a sysadmin allowlist has to be built in
   two places and kept in sync.

## What "solved" looks like (restated, now grounded)

A first-class **authorised-operator mode** for Claude Code in which:

- Trust scope is declared per operator and per set of machines /
  directories, not derived from cwd.
- The mode is stable across releases — not a flag whose semantics
  change every few versions.
- Declarative filesystem scope and Bash permission scope are unified
  or at least coherently linked.
- MCP's "persistent, operator-granted scope" property is generalised
  to local shell operations rather than being the workaround that
  happens to work.
- The use case is acknowledged upstream, so changes to the default
  sandbox are evaluated against this pattern as well as the
  developer-in-a-repo pattern.

Deep-dives 1 and 2 will take the MCP escape hatch and the hooks/settings
layer respectively, and evaluate how far each can carry the pattern
today — and where upstream change is genuinely required.

See also `notes/260421-no-logical-cwd-and-meta-defense.md` for two
follow-on points: (a) the "no logical cwd for general machine work"
sub-problem, to be added to the deep-dive queue as a session-anchoring
question, and (b) the meta-defense of the use case against the
"wrong tool for the job" critique.

[^auto-mode]: `sources/260421-cc-auto-mode-engineering-blog.md`
[^modes]: `sources/260421-cc-permission-modes-docs.md`
[^settings]: `sources/260421-cc-settings-docs.md`
[^9184]: `sources/260421-gh-issue-9184-skip-perms-root.md`
[^3490]: `sources/260421-gh-issue-3490-root-flag-request.md`
[^36168]: `sources/260421-gh-issue-36168-bypass-broken-v2178.md`
[^1135]: `sources/260421-gh-issue-1135-sudo-askpass.md`
[^xda]: `sources/260421-xda-claude-code-mcp-homelab.md`
[^jmagar]: `sources/260421-jmagar-claude-homelab.md`
[^alderson]: `sources/260421-alderson-sysadmin-post.md`
[^larcombe]: `sources/260421-larcombe-sysadmin-post.md`
[^ricardodecal]: `sources/260421-ricardodecal-sudo-guide.md`
