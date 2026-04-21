---
url: https://www.xda-developers.com/connected-claude-code-through-mcp-manage-entire-lab-by-talking/
title: "I connected Claude Code to my home server through MCP, and now I manage my entire lab by talking to it"
type: secondary
captured: 2026-04-21
tags: [mcp, homelab, docker-mcp, mcp-mediated-admin]
---

XDA writeup of a homelab wired into Claude Code via `docker-mcp`. Notable for this research
because the author's operations flow through an MCP server rather than raw shell — container list
/ restart / Postgres query / diagnostics — which sidesteps much of the Bash permission layer.
Concrete evidence of MCP as a parallel privileged channel that the shell sandbox doesn't fully
inspect, which is exactly the workaround pattern flagged in the workspace's framing.
