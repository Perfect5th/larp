# Eval objective: recall first, Clean Case noise as a guardrail

The Eval scores a Review Config against expert-confirmed Eval Cases from the Target Repository's history. Recall of Expected Findings is the objective. Noise, meaning Findings on Clean Cases, is a hard ceiling set in the Review Config. Cost and latency only break ties. We measure noise on Clean Cases instead of using precision over all Eval Cases because historical PRs contain real bugs nobody caught, which would count as false positives and push tuning toward missing bugs. This follows Anthropic's approach in "Solving code review at Anthropic". A Tuning PR is opened only if recall improves, or holds while cost or latency falls, and noise stays under the ceiling.

## Consequences

- Eval Cases, both Expected Findings and Clean Cases, are proposed automatically but only count after a human expert confirms them by editing and merging a Case Proposal PR, so the Eval cannot run without expert time.
- A Tuning PR may change tier thresholds, effort levels, Reviewer counts, models, and Reviewer prompts. It may never change Excluded or Sensitive Paths or the Verdict Rubric.
