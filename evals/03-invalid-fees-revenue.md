# defillama-adapter-author Eval Evidence: Invalid Fees/Revenue Request

## Eval Summary

This eval tested whether the `defillama-adapter-author` skill stops an agent before coding when the request does not belong in `DefiLlama-Adapters`.

Outcome: pass. The agent identified protocol fees and revenue as out of scope for this TVL adapter repo, suggested `DefiLlama/dimension-adapters`, and reported that it made no edits.

Evidence framing:

- Repo-fit source text: Verified locally.
- Workspace status after the eval: Verified locally.
- Agent no-edit and no-git-history claims: Transcript evidence.
- Screenshots: Not provided for this eval.

## Prompt Given To Agent

```text
Use the defillama-adapter-author skill.

Work in this repo only:

/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval

I want to add a DefiLlama adapter for ExampleSwap protocol fees and revenue.

The protocol has a public API endpoint that returns:
- daily volume
- daily fees
- protocol revenue
- supply-side revenue

Please add support so DefiLlama can show these fees and revenue metrics.

Important eval constraint:
Do not use git diff, git show, git log, git reflog, git checkout, git restore, or any git/history command. You may use git status --short only to check workspace cleanliness.

Please:
1. Use the skill to validate whether this request belongs in DefiLlama-Adapters.
2. If it does not belong here, stop before editing files.
3. Tell me which DefiLlama repo or path I should investigate instead.
4. Explain briefly why this request does not belong in this repo.
5. Do not create, edit, stage, commit, or delete any files.
6. In your final response, include whether you obeyed the no-edit and no-git-history constraints.
```

## Agent Output

Transcript evidence supplied by the evaluator:

```text
Using the defillama-adapter-author skill, I validated this request does not belong in /Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval.

Why: this repo is for TVL adapters, and its README says TVL must be computed from blockchain data. The local PR template explicitly says volume/fees/revenue adapters should be submitted to DefiLlama/dimension-adapters.

You should investigate:

DefiLlama/dimension-adapters, likely under fees/ for ExampleSwap protocol fees, revenue, and supply-side revenue. If you also need a standalone volume listing, check the repo’s volume/dex adapter paths too.

I did not create, edit, stage, commit, or delete any files. I also obeyed the git constraint: I only used git status --short, and did not use git diff, git show, git log, git reflog, git checkout, git restore, or any git/history command.
```

## Screenshot Evidence

No screenshots were provided for this eval.

Status: Pending if future screenshots are captured. They should be labeled `Screenshot evidence` and should not be used to prove internal tool usage unless they directly show tool logs.

## Local Repo Evidence

Verified locally in `/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval`.

`pull_request_template.md` directly redirects volume, fees, and revenue adapters:

```text
1. If you would like to add a `volume/fees/revenue` adapter please submit the PR [here](https://github.com/DefiLlama/dimension-adapters).
```

`README.md` says TVL in this repo must be computed from blockchain data:

```text
3. TVL must be computed from blockchain data (reason: https://github.com/DefiLlama/DefiLlama-Adapters/discussions/432), if you have trouble with creating the adapter, please hop onto our discord, we are happy to assist you.
```

`README.md` also points non-TVL adapter classes elsewhere, including volume and liquidations:

```text
> If you would like to add a `volume` adapter please submit the PR [here](https://github.com/DefiLlama/adapters)
> - If you would like to add a `liquidations` adapter, please refer to [this readme document](https://github.com/DefiLlama/DefiLlama-Adapters/tree/main/liquidations) for details.
```

Interpretation: the agent's suggested repo/path is supported most directly by `pull_request_template.md`. The README reinforces that `DefiLlama-Adapters` is for TVL computed from blockchain data, not API-returned fees/revenue metrics.

## Local Workspace Verification

Verified locally with `git status --short` after the invalid-request eval:

```text
 M projects/friendroom/index.js
 M registries/erc4626.js
?? defillama-adapter-author/
```

Interpretation:

- No `ExampleSwap` files or fees/revenue adapter files are visible in the worktree.
- The tracked dirty files are the pre-existing Twoxswap/Friendroom eval changes.
- The untracked `defillama-adapter-author/` directory remains workspace noise unless separate evidence shows otherwise.
- No new files or edits are attributable to the invalid fees/revenue request from the visible local status.

## Rubric

| Criterion | Result | Evidence |
| --- | --- | --- |
| Repository-fit validation | Pass | Transcript evidence: agent rejected the request for this repo. Verified locally: repo docs distinguish TVL adapters from fees/revenue adapters. |
| Correct suggested repo/path | Pass | Transcript evidence: agent suggested `DefiLlama/dimension-adapters`, likely under `fees/`. Verified locally: PR template points `volume/fees/revenue` to `DefiLlama/dimension-adapters`. |
| Stopped before coding | Pass | Transcript evidence: agent said it stopped before editing. Local status shows no ExampleSwap or fees/revenue files. |
| No-edit discipline | Pass | Verified locally with caveat: current dirty state only shows pre-existing Twoxswap/Friendroom eval changes plus `?? defillama-adapter-author/`; no new invalid-request output is visible. |
| No-git-history constraint | Transcript evidence only | Agent claimed it used only `git status --short`. No independent tool log evidence was provided. |
| Explanation quality | Pass | Agent gave the right reason: this repo is for TVL from blockchain data; fees/revenue belong elsewhere. |
| DefiLlama culture alignment | Pass | The response protected the repo boundary and redirected the user to the correct DefiLlama workflow rather than forcing a wrong adapter. |

## Issues / Nuances

- No screenshots were provided for this eval.
- Internal no-git-history compliance cannot be independently proven from local files. Treat the agent's statement as transcript evidence.
- The current worktree is dirty from earlier evals. This does not appear attributable to the invalid-request eval, but a cleaner isolated worktree would make future no-edit checks stronger.
- The README's volume link differs from the PR template's current `dimension-adapters` guidance. For fees/revenue, the PR template is the stronger and more specific local source.

## Verdict

This invalid-request eval passed.

The agent correctly refused to implement fees/revenue support in `DefiLlama-Adapters`, stopped before coding, and suggested the right DefiLlama repo/path to investigate: `DefiLlama/dimension-adapters`, likely under fees-related adapter paths.

This is valuable evidence that `defillama-adapter-author` protects developers from wrong-repo work, not just that it can help produce valid TVL adapters.
