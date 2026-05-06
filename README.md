# DefiLlama TVL adapter-author eval evidence

This repo records three evals for the `defillama-tvl-adapter-author` skill.

The skill is meant for agents working in `DefiLlama/DefiLlama-Adapters`.

Name note: the evals were run while the skill still used its original name, `defillama-adapter-author`. It was renamed to `defillama-tvl-adapter-author` before PR review to make the TVL scope explicit.

These evals check whether an agent can:

- validate whether a request belongs in `DefiLlama-Adapters`
- choose repo-native adapter patterns and helpers
- avoid wrong-repo work for fees, revenue, volume, listing-only, or liquidations requests
- run the appropriate `node test.js ...` validation command
- prepare PR metadata without inventing unknown facts
- avoid package/lockfile changes and other forbidden actions

## Evals

| Eval | Scenario | Result | Evidence note |
| --- | --- | --- | --- |
| 1 | Twoxswap ERC4626 vault TVL authoring | Pass | [01-twoxswap-erc4626.md](evals/01-twoxswap-erc4626.md) |
| 2 | Friendroom native ETH owner-balance TVL authoring | Pass | [02-friendroom-native-eth.md](evals/02-friendroom-native-eth.md) |
| 3 | Invalid fees/revenue request repo-fit gate | Pass | [03-invalid-fees-revenue.md](evals/03-invalid-fees-revenue.md) |

## Fixture design

We picked Twoxswap and Friendroom because they already existed in `DefiLlama-Adapters`.

That gave us a known answer to compare against.

Before running the agents, we made a separate eval worktree and removed those implementations:

- Twoxswap was removed from `registries/erc4626.js`.
- `projects/friendroom/index.js` was deleted.

The agents received protocol facts in the prompt.

They were told not to use git/history commands such as `git diff`, `git show`, `git log`, `git checkout`, or `git restore`.

They could inspect current repo files, helpers, registries, examples, and run `node test.js`.

The goal was not byte-for-byte restoration.

The goal was to see whether the skill pushed the agent toward a correct repo-native implementation.

The screenshots in `artifacts/screenshots/` show the removed and added code context for the two adapter evals.

No-git-history compliance is transcript evidence unless direct tool logs are available.

## High-level verdict

All three evals passed.

The two adapter evals produced working TVL implementations and passed `node test.js`.

They were not exact copies of the original code. That is fine. The useful signal is that they followed DefiLlama patterns and validated cleanly.

The invalid-request eval stopped before editing.

It treated protocol fees and revenue as out of scope for `DefiLlama-Adapters` and pointed to `DefiLlama/dimension-adapters`.

That matters. A good skill should prevent wrong-repo work, not just help write adapters.

## Evidence labels

The notes separate evidence by strength:

- `Verified locally`: checked from local files, worktree status, or commands run while building this pack
- `Transcript evidence`: provided agent output or conversation excerpts
- `Screenshot evidence`: provided screenshots stored under `artifacts/screenshots/`
- `Uncertain` or `Pending`: not proven from the evidence we have

Internal no-git-history compliance stays transcript evidence unless direct tool logs are available.

## Repository layout

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
    └── defillama-tvl-adapter-author/
```

## Skill snapshot

`skill-snapshot/` contains the skill files that were evaluated.

The source of truth remains the `DefiLlama-Adapters` PR branch.

The snapshot lets reviewers read the instructions next to the eval findings.
