---
title: Addendum — the "no logical cwd" problem, and a meta-defense of the use case
captured: 2026-04-21
kind: note
status: raw
relates_to: outputs/problem-statement-260421.md
---

# Addendum to the problem statement

Two further points to fold into the research. Both came up after the
initial problem statement was solidified, and both belong in the record
before they get lost.

## 1. There is no logical cwd for "work on my computer"

The problem statement frames cwd-as-trust-unit as the load-bearing design
choice. The lived corollary of that — worth surfacing as its own
challenge — is this:

**When the task is "generally work on my machine," there is no obvious,
correct working directory to launch Claude Code from.**

Concretely:

- Sysadmin work doesn't have a project root (already covered).
- But even *choosing where to start the session* is an unsolved UX
  problem. The current workaround is to open at `$HOME` so the session
  has at least nominal exposure to everything beneath it.
- This does not actually escape the cwd limitation — it just makes the
  cwd maximally broad. The harness still treats `~/` as the perimeter,
  and operations outside it (`/etc`, `/var`, other users' homes, mounted
  volumes, remote hosts) are still elevated.
- It also creates conceptual friction: the session *feels* constrained
  to a place because that's how terminals work, but there is no
  *logical* place for it to be constrained to. The directory choice is
  arbitrary, which is a sign the abstraction isn't fitting the task.

This is a second-order symptom of the cwd-as-trust-unit choice, but it
deserves to be explored on its own terms. A durable "authorised-operator
mode" would need an answer to *"where do I launch Claude Code when the
answer is: everywhere I own"* — probably some form of operator-scoped
session that doesn't require picking a directory at all, or that treats
cwd as a navigation hint rather than a trust boundary.

**Action:** add to deep-dive queue as a distinct sub-question —
"Session anchoring for non-project work: what should replace cwd as the
launch context?"

## 2. Meta-defense — why this use case is worth taking seriously

A fair critique of this whole research direction is: *"You're using
Claude Code the wrong way. It wasn't built for this."* Both halves of
that critique are technically defensible. The response for the record:

- **Every agent and tool in this category has been tried.** The
  judgement that Claude is in a league of its own for this kind of work
  is not casual — it's comparative. The alternative tools either don't
  have the reasoning quality, don't have the tool-use discipline, or
  don't have the integration surface.
- **"Not what it was built for" is a framing, not a verdict.** The more
  productive framing is: *this is a high-value use case that hasn't
  been factored into the product's design consideration yet.* Giving up
  on it because the tool is too good at it is the wrong trade.
- **Empirical track record.** This pattern has been in active use for
  roughly a year, and it has improved monotonically as Claude has
  improved. Concrete outcomes include:
  - Countless bug fixes across the workstation.
  - Security audits and remediations.
  - Package-state reconciliation.
  - Firewall repair.
  - Building a new home server.
  - A seamless migration from the old server to the new one.
- **The risk rhetoric outruns the lived evidence.** In the year of use
  above, nothing catastrophic has happened. That is a real data point
  against the implicit premise of the default safety model — that this
  class of use is so dangerous it must be continuously narrowed.

This is not an argument that the safety model is wrong in general. It
is an argument that the cost/benefit balance for a single authorised
operator working on their own machines is different from the balance
for the default developer-in-a-repo premise, and the lived evidence
supports treating this use case as first-class rather than edge-case.

**Action:** this is rationale, not a research question — keep it
attached to the problem statement so the motivation is preserved
alongside the findings.
