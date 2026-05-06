# DefiLlama Adapter Author Eval Evidence

This repository contains the evidence pack for evaluating the `defillama-adapter-author` agent skill for `DefiLlama/DefiLlama-Adapters`.

The evals focus on whether the skill helps an agent behave like a good DefiLlama adapter contributor:

- validate whether a request belongs in `DefiLlama-Adapters`
- choose repo-native adapter patterns and helpers
- avoid wrong-repo work for fees, revenue, volume, listing-only, or liquidations requests
- run the appropriate `node test.js ...` validation command
- prepare PR metadata without inventing unknown facts
- avoid package/lockfile changes and other forbidden actions

## Evals

| Eval | Scenario | Result | Evidence note |
| --- | --- | --- | --- |
| 1 | Twoxswap ERC4626 vault TVL reconstruction | Pass | [01-twoxswap-erc4626.md](evals/01-twoxswap-erc4626.md) |
| 2 | Friendroom native ETH owner-balance TVL reconstruction | Pass | [02-friendroom-native-eth.md](evals/02-friendroom-native-eth.md) |
| 3 | Invalid fees/revenue request repo-fit gate | Pass | [03-invalid-fees-revenue.md](evals/03-invalid-fees-revenue.md) |

## High-Level Verdict

All three cleaner evals passed.

The two valid-adapter evals show the skill steering agents toward functionally correct, repo-style-valid TVL implementations with successful `node test.js` validation. Exact golden reconstruction is not the goal; correct DefiLlama-native behavior is.

The invalid-request eval is especially important: the agent stopped before editing, identified protocol fees/revenue as out of scope for `DefiLlama-Adapters`, and redirected to `DefiLlama/dimension-adapters`. This is useful evidence that the skill protects developers from wrong-repo work, not only that it helps write TVL adapters.

## Evidence Labels

The notes distinguish evidence strength:

- `Verified locally`: confirmed from local files, worktree status, or commands run during evidence-pack creation
- `Transcript evidence`: provided agent output or conversation excerpts
- `Screenshot evidence`: provided screenshots stored under `artifacts/screenshots/`
- `Uncertain` or `Pending`: not independently proven from available evidence

Internal no-git-history compliance is treated as transcript evidence unless direct tool logs are available.

## Repository Layout

```text
.
├── README.md
├── artifacts/
│   ├── README.md
│   └── screenshots/
├── evals/
│   ├── 01-twoxswap-erc4626.md
│   ├── 02-friendroom-native-eth.md
│   └── 03-invalid-fees-revenue.md
└── skill-snapshot/
    ├── README.md
    └── defillama-adapter-author/
```

## Skill Snapshot

`skill-snapshot/` contains a local copy of the skill files that were evaluated. The source of truth remains the `DefiLlama-Adapters` PR branch; the snapshot is included here so reviewers can inspect the evaluated instructions alongside the eval findings.

