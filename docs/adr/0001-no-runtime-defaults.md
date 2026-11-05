# No runtime defaults: the Review Config lives entirely in the Target Repository

All review behaviour must be derivable from the Target Repository, so LARP ships no runtime defaults for models, prompts, Reviewers, thresholds, paths, or the Verdict Rubric. Instead, a scaffold command copies a starter Review Config into the Target Repository once, after which the repository owns it. We rejected "package defaults plus repo overrides" because a LARP upgrade could then silently change review behaviour and make Eval results incomparable over time.

## Consequences

- Behaviour can only change through a PR in the Target Repository: edits to the Review Config, a Tuning PR, or a bump of the pinned LARP version. LARP in turn pins an exact OpenCode version.
- Improvements to the scaffolded starter config do not reach existing repositories automatically.
- The scaffold defaults to advisory blocking mode with approving off. Blocking and approving are each an explicit opt-in. Approving also requires the GitHub setting "Allow GitHub Actions to create and approve pull requests".
