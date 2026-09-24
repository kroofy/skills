---
name: upgrade-to-simplify
description: Find code and configuration a dependency upgrade lets us remove or delegate upstream. Use when evaluating upgrade-enabled simplifications or delivering them as focused PRs.
---

# Upgrade to Simplify

Use a dependency upgrade to reduce the responsibilities this codebase owns.
Prefer less maintained code, fewer concepts and dependencies, and simpler
configuration while preserving required behavior.

Review application code, tests, configuration, build tooling, and CI.
Follow the requested scope: recommend, implement, or publish.
Publish only when authorized.

## 1. Establish the delta

Identify the dependency, exact current → target versions, companion packages,
and runtime requirements. Establish the intended base branch and whether the
upgrade has merged; link its PR or commit when available.

Read relevant release notes, migration guides, and versioned API documentation.
Inspect upstream source when documented behavior is unclear.

Done when the version comparison, base, and supporting sources are explicit.

## 2. Find worthwhile simplifications

Trace changed dependency capabilities into actual repository usage. Look for:

- Wrappers, adapters, polyfills, and workarounds replaced upstream.
- Configuration that repeats defaults.
- Custom behavior covered by upstream primitives.
- Redundant libraries or execution environments.
- APIs that reduce existing branching or state.

Evaluate net maintenance cost, including replacement configuration and changed
semantics. Keeping a small existing abstraction may be simpler.

For each worthwhile opportunity, record:

- Goal, affected files, and current responsibility.
- Upstream replacement and version-specific evidence.
- What code, configuration, or dependency disappears.
- Behavioral implications and required verification.

Distinguish upgrade requirements, optional simplifications, and experiments.
Label capabilities that predate the upgrade.

Done when every recommendation is grounded in repository usage and an
available upstream capability. No worthwhile opportunities is a valid result.

## 3. Scope the changes

Present a short, prioritized list using the opportunity records.

Each proposed PR should achieve one coherent goal and be independently
understandable, verifiable, and revertible. Group edits needed for that goal;
separate changes whose goals or validation differ.

Add the base, PR dependencies, and verification plan to each selected record.
Base independent PRs on the latest intended base branch. Stack only for a real
dependency, such as an unmerged upgrade, and state it explicitly.

Done when selected changes have clear boundaries and verification plans.
For a recommendations-only request, return the list and stop.

## 4. Implement, verify, and deliver

Establish existing behavior before editing. Preserve meaningful assertions
and exercise the actual integration, including companion packages.

When parallelizing independent changes, use subagents in separate worktrees
with explicit ownership, bases, and completion checks. Review their diffs
and verification evidence.

Support performance claims with repeated baseline/candidate runs using the
same workload. Record runtime, resource use, and failures; re-measure if the
workload changes.

Classify check failures as reproduced regressions, reproduced pre-existing
failures, or unresolved. A peer-range mismatch alone does not establish runtime
failure; a passing rerun alone does not establish that a failure is unrelated.
Keep speculative fixes separate.

Retain changes whose benefit is supported. A change is verified when its
required checks pass; otherwise report the blocker and outstanding checks.

Summarize delivered changes and verification results. When publication is
authorized, use each opportunity record as the PR description, updated with
actual results, and return PR links. Briefly note rejected experiments and
unresolved findings.
