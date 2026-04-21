---
url: https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md
title: "Claude Code CHANGELOG.md"
type: changelog
captured: 2026-04-21
tags: [changelog, sandbox, permission-hardening, regressions]
---

Upstream changelog. Shows a sustained pattern of sandbox/permission hardening across 2025–2026:
fixes for permission bypasses via backslash-escaped flags, compound Bash commands, env-var
prefixes, `/dev/tcp` redirects, piped `cd` segments downgrading deny rules; addition of
PID-namespace subprocess sandboxing on Linux (`CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`), per-session
script caps, `sandbox.filesystem.allowRead` carve-outs, and `sandbox.network.deniedDomains`.
Important for the research narrative: every hardening entry is a correct security fix **and** a
potential regression against a sysadmin workflow that depended on the previous looseness. Name
specific versions when citing.
