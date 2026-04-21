# Sources index

Initial sourcing pass, captured 2026-04-21.

| Filename | Type | Title | Relevance |
|---|---|---|---|
| `260421-cc-permission-modes-docs.md` | docs | Choose a permission mode (Claude Code Docs) | Canonical reference for default/acceptEdits/plan/auto/dontAsk/bypassPermissions + protected paths. |
| `260421-cc-settings-docs.md` | docs | Claude Code settings (Claude Code Docs) | settings.json hierarchy, allow/deny/ask, `additionalDirectories`, sandbox fs/network block. |
| `260421-cc-hooks-docs.md` | docs | Hooks reference (Claude Code Docs) | PreToolUse / PermissionRequest interception — basis for user-owned authorisation layer. |
| `260421-cc-auto-mode-engineering-blog.md` | primary | Claude Code auto mode (Anthropic Engineering) | Anthropic's framing of "outside the project directory" as the high-scrutiny category. |
| `260421-cc-changelog.md` | changelog | Claude Code CHANGELOG | Evidence of continuous sandbox/permission hardening and the sysadmin regression pattern. |
| `260421-gh-issue-9184-skip-perms-root.md` | community | Issue #9184: skip-perms blocked under root/sudo | Regression narrowing the sledgehammer flag against a canonical sysadmin case. |
| `260421-gh-issue-3490-root-flag-request.md` | community | Issue #3490: alt flag for root-in-container | "Closed not planned" = upstream product decision against sysadmin escape hatch. |
| `260421-gh-issue-36168-bypass-broken-v2178.md` | community | Issue #36168: bypassPermissions broken after v2.1.77 | Pins a version boundary where the sledgehammer itself silently narrowed. |
| `260421-gh-issue-1135-sudo-askpass.md` | community | Issue #1135: SSH-based sudo askpass helper | Sudo-in-automation pain point; Anthropic declines to bless a pattern. |
| `260421-alderson-sysadmin-post.md` | secondary | Alderson — How I use Claude Code for sysadmin | Workflow pattern: per-host folders, SSH aliases, bastion, infra-as-markdown. |
| `260421-larcombe-sysadmin-post.md` | secondary | Larcombe — Claude Code for Server Administration | "Diagnostic partner not executor" framing; counter to pure bypass workflows. |
| `260421-xda-claude-code-mcp-homelab.md` | secondary | XDA — Homelab via docker-mcp | MCP as parallel privileged channel that bypasses the shell permission layer. |
| `260421-ricardodecal-sudo-guide.md` | secondary | Running sudo in Claude Code (SUDO_ASKPASS) | Minimal practical mitigation for non-interactive sudo. |
| `260421-jmagar-claude-homelab.md` | community | jmagar/claude-homelab | External reference implementation of skills/hooks/agents for homelab admin. |
| `260421-open-interpreter-contrast.md` | community | Open Interpreter | Contrast tool — ops-oriented framing vs. Claude Code's dev-tool default. |

## Gaps noted

- **Official Anthropic positioning of Claude Code as sysadmin-friendly.** Deliberately absent from
  public docs; all first-party framing is developer-focused. Documented by absence rather than
  any single link.
- **r/ClaudeAI threads on sysadmin frustration.** Search surfaced mentions but no single
  canonical high-signal thread worth a standalone entry; revisit during deep-dive if needed.
- **Enterprise/MDM managed-settings patterns for authorised-operator profiles.** Referenced only
  indirectly via `disableBypassPermissionsMode` / `disableAutoMode`; no public example of an
  admin profile configured *for* sysadmin use rather than against it.
- **A primary Anthropic statement on remote/SSH use.** None found — the docs treat the local cwd
  as the universe.
