# Verdicts gate merges; pushes make Verdicts stale; Re-review is on demand

A Verdict may block merging; overrides use GitHub's existing admin bypass rather than a "break glass" command. Re-review only happens when a human asks for it, to avoid the runaway agent-author/agent-reviewer loops Anthropic reported (50+ rounds on one PR). Combining gating with on-demand review means any push makes the current Verdict stale: a stale block keeps blocking and a stale approval stops counting until a Re-review replaces it. We rejected automatic re-review on every push (Cloudflare's model) because of cost and non-convergence.

## Consequences

- An author who fixes a blocking Finding must also request a Re-review before the PR can merge.
- A Re-review recalculates the Review Tier on the PR's whole current diff and receives its previous Findings. Fixed ones are resolved, unfixed ones are re-emitted, and threads a human resolved are respected unless the issue got worse.
- Blocking uses a "request changes" review, enabled by the Review Config's blocking mode (the alternative, advisory mode, only comments). For blocking to work, the Target Repository must require approving reviews on important branches and restrict who can dismiss reviews. GitHub keeps change requests in force across pushes and can dismiss stale approvals, which gives Stale Verdict behaviour without extra LARP logic. Because this is a review rather than a required check, a review that never runs (e.g. the workflow is removed, or the Review Config is missing or invalid) does not block. LARP fails open. CODEOWNERS on `.github/workflows/`, the Review Config, and `CODEOWNERS` itself guards against PRs that disable review.
- The Review Config is always read from the base branch, so a PR cannot weaken the review that judges it.
