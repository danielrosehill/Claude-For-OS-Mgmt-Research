---
title: Usage pattern — Claude Code as a general-purpose OS/admin harness
type: framing
captured: 2026-04-21
owner: Daniel Rosehill
---

# The usage pattern being researched

This document is the anchor framing for the workspace. Every deep-dive and
output should treat it as the ground truth about *how* the user actually uses
Claude Code, and *where* the friction sits.

## The user's starting point

- Long-time Linux desktop user (currently Ubuntu 25.10 + KDE Plasma on Wayland).
- Pre-existing interest in "AI returning to the terminal" — experimented with
  Open Interpreter and similar computer-use tools before settling on Claude Code.
- Runs a personal tech estate that spans more than a single dev box: desktop
  workstation, home server, Home Assistant, LAN devices (Raspberry Pi hosts,
  NAS, router), plus VPS / cloud hosts.

## How Claude Code is actually used

Claude Code is used as a **general-purpose OS and infrastructure harness**, not
primarily as developer tooling. Concrete use cases, in rough order of frequency:

1. **Desktop Linux administration** — debugging, installing/removing packages,
   fixing broken state, troubleshooting audio/display/KDE/Wayland, hardware
   probes. Has made running Linux on the desktop substantially easier.
2. **Home server / Home Assistant / LAN ops** — service debugging, backup job
   setup, config edits, Zigbee/MQTT troubleshooting, network scans.
3. **VPS / cloud server administration** — same shape of work, remote. The user
   views local-vs-remote as an **immaterial distinction** for this pattern;
   both are "a shell on a machine I own."
4. **Non-repo "folder as workspace"** — planning workspaces, diaries,
   research workspaces (like this one), media pipelines, document generation.
   The repository abstraction is used as a generic directory of state, not
   primarily as a codebase.
5. **MCP-mediated operations** — progressively more work flows through MCP
   servers (Home Assistant, LAN devices, Google Workspace, 1Password, n8n,
   Playwright, etc.) rather than raw shell.
6. **Development** — still present, but one use case among many and not the
   framing lens.

## The core tension

Anthropic builds and positions Claude Code primarily as **developer tooling**.
The default safety model — project-scoped sandboxing, permission prompts per
tool call, write restrictions to the working directory — fits that framing
cleanly.

For the user's pattern, that same safety model becomes **load-bearing friction**:

- OS-management work inherently touches paths *outside* the project folder
  (`/etc`, `/var`, `~/.config`, `/usr/local`, systemd units, user services).
- Administration requires `sudo` and privileged operations that the sandbox
  is designed to gate.
- Remote work (SSH, MCP servers for LAN hosts, VPS) blurs the idea of a
  "project directory" entirely — the relevant state lives on another machine.
- Non-code workspaces (diaries, planners, research) still need to read/write
  across `~/` broadly, not just the repo they were launched from.

The user's stance: the safety mechanisms are **well-intended and correct for
their original target use case**, but they are mis-fit for sysadmin use, and
each tightening of sandbox defaults in a Claude Code release creates a
parallel regression in the sysadmin workflow. The user is not trying to
disable oversight — they still approve and reject tool calls and monitor the
session. What they want is to stop feeling like they are *fighting* the
harness to do legitimate, authorised work on their own machines.

## Current workarounds (to be documented and improved)

Both of the following are in active use:

1. **`--dangerously-skip-permissions`** — the sledgehammer. Reliable but
   framed (by name and by Anthropic messaging) as a last resort, and gets
   disabled or narrowed in periodic harness updates.
2. **Broad `allow` lists in `settings.json`** — per-workspace and user-level
   permission allowlists, plus `permissions.defaultMode`, to pre-approve the
   sysadmin-shaped tool calls that would otherwise prompt every time.

Likely also in the mix (to verify during sourcing):

- Hook scripts that pre-authorise or re-shape tool calls.
- Choice of working directory (launching Claude from `~/` or a parent dir)
  to widen the de facto sandbox scope.
- MCP servers that route privileged operations through a channel the shell
  sandbox doesn't inspect (e.g., Home Assistant MCP, LAN device MCP servers
  that `run_command` on remote hosts).
- The `skills/` and `agents/` layer (workstation-manager, sysadmin-homelab,
  desktop-manager) which encode standard admin recipes.

## What "solved" would look like

- A first-class mode or profile for **authorised-operator on their own
  machines**, distinct from the default project-sandboxed mode.
- Stability of that mode across Claude Code releases — not a flag that gets
  renamed, narrowed, or deprecated every few versions.
- Durable, declarative allowlists that survive upgrades and can be shared
  across workspaces.
- Ideally, an upstream acknowledgement that OS/infrastructure administration
  is a legitimate first-class use case of Claude Code — not a misuse of a
  dev tool.

## Scope of this research workspace

- **In scope:** local desktop admin, LAN/home-server admin, VPS/cloud admin,
  MCP-mediated admin, non-code workspace patterns, sandbox/permission model,
  harness-level mitigations (settings.json, hooks, skills, MCP scoping).
- **Out of scope (for now):** model behaviour / prompting, application-level
  development workflows, enterprise/team deployments of Claude Code.

## Output expectations

Deep-dives should:

- Distinguish **Claude Code harness behaviour** (sandbox, permissions, hooks)
  from **Anthropic product positioning** (docs, marketing, default UX).
- Name the specific version / changelog entries when citing sandbox changes.
- Where a workaround is fragile, flag why and what would replace it.
- Treat local and remote admin as the same class of problem unless a
  finding genuinely only applies to one.
