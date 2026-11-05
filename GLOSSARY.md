# LARP

An agentic code-review system that runs on GitHub pull requests, configured entirely from within the repository being reviewed.

## Language

**LARP**:
The reusable code-review package that a Target Repository invokes from its GitHub workflows, pinned to an exact version. LARP itself pins an exact harness version.
_Avoid_: The action, the bot, the tool

**Target Repository**:
The repository whose pull request is being reviewed; it is the sole source of all review configuration, including which secrets to use.
_Avoid_: Host repo, consumer repo, client repo

**Review Config**:
The complete, explicit definition of how reviews run for a Target Repository — models, effort levels, Reviewers and their responsibilities, tier thresholds, Excluded and Sensitive Paths, Verdict permissions, and Eval settings. Nothing is inherited from outside it.
_Avoid_: Settings, defaults, profile

### Findings

**Finding**:
A single issue raised by a reviewer about a specific part of a pull request's changes.
_Avoid_: Comment, issue, flag

**Severity**:
How serious a Finding is: **critical**, **warning**, or **suggestion**. Together with the Verdict Rubric it decides the Verdict.
_Avoid_: Priority, level, importance

### Eval

**Eval**:
An opt-in, separately-run assessment of review quality, graded against historical pull requests in the Target Repository.
_Avoid_: Benchmark, test run

**Tuning PR**:
A pull request opened by the Eval that proposes changes to the Target Repository's Review Config.
_Avoid_: Auto-tune commit, config update

**Case Proposal PR**:
A pull request opened by the Eval that proposes new Eval Cases; an expert confirms them by editing and merging it.
_Avoid_: Labeling PR, eval set update

**Expected Finding**:
A Finding known to be correct for a historical pull request, derived from human review comments or later evidence (follow-up fixes, reverts) and confirmed by a human expert.
_Avoid_: Ground truth, label, golden comment

**Eval Case**:
A historical pull request paired with its Expected Findings, used by the Eval.
_Avoid_: Fixture, sample

**Clean Case**:
An Eval Case confirmed by a human expert to have no Expected Findings; any Finding reported on it counts as noise.
_Avoid_: Negative case, control PR

### Tiering

**Review Tier**:
The level of review a pull request receives — **Trivial**, **Light**, or **Full** — determining which reviewers run. Chosen by whichever of files-changed or lines-changed indicates the higher tier.
_Avoid_: Risk tier, review size, Lite

**Excluded Path**:
A file pattern whose changes are neither counted when choosing the Review Tier nor shown to reviewers (e.g. lockfiles, generated code). Takes precedence over Sensitive Path.
_Avoid_: Noise file, ignored file

**Sensitive Path**:
A directory where any change forces a Full review regardless of size.
_Avoid_: Security-sensitive file, protected path

### Agents

**Coordinator**:
The agent that runs on every review, spawns the Reviewers for the Review Tier, and consolidates their Findings into a single review.
_Avoid_: Orchestrator, judge, lead agent

**Reviewer**:
A specialist agent with one area of responsibility (e.g. security, performance) that produces Findings for the Coordinator.
_Avoid_: Subagent, sub-reviewer, specialist

### Outcomes

**Verdict**:
The Coordinator's overall decision on a pull request. In blocking mode it may block merging and, if the Target Repository opts in, may approve; in advisory mode it is only a comment.
_Avoid_: Result, status, decision

**Verdict Rubric**:
The rules in the Review Config that map a set of Findings by Severity to a Verdict. Set only by humans, never by a Tuning PR.
_Avoid_: Policy, approval rules

**Stale Verdict**:
A Verdict issued on an earlier commit of a pull request that has since received new commits. A stale block still blocks; a stale approval no longer counts. Only a Re-review replaces it.
_Avoid_: Outdated review, expired verdict

**Re-review**:
A human-requested repeat review of a pull request that already has a Verdict.
_Avoid_: Incremental review, follow-up review
