# defillama-adapter-author Eval Evidence: Friendroom

## Eval Summary

This eval tested whether the `defillama-adapter-author` skill helped an agent add Friendroom TVL support in `DefiLlama-Adapters` without using git history to rediscover the previous implementation.

Outcome: functionally correct. The agent created a simple native ETH owner-balance adapter, ran the correct `node test.js projects/friendroom/index.js` validation command, and produced PR metadata with unknowns left as TODOs.

Evidence framing:

- Functional result: Verified locally.
- Workflow claims: Transcript evidence.
- Code-change screenshot: Screenshot evidence.
- Internal no-git-history compliance: Transcript evidence only, not independently proven.

Important nuance: exact golden reconstruction is not the goal for this eval. The agent used `sumTokensExport({ owner, tokens: [nullAddress] })` instead of the original explicit async `sumTokens2` implementation. This is equivalent and valid repo style for a simple owner/native-token balance adapter.

## Skill Version / Source

- Skill name: `defillama-adapter-author`
- Source repo: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters`
- Source repo HEAD during note update: `2b5d8bcb6`
- Skill path: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters/skills/defillama-adapter-author/SKILL.md`
- Eval worktree: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval`
- Evidence note date: 2026-05-06
- Version: local draft skill, not a published/tagged release

## Prompt Given To Agent

```text
Use the defillama-adapter-author skill.

Work in this repo only:

/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval

I want to add Friendroom to DefiLlama TVL.

Friendroom has an Ethereum contract that holds ETH. TVL should count the ETH balance held by this contract.

Known details:
- Protocol: Friendroom
- Chain: Ethereum
- Contract address: 0x9BD0474CC4F118efe56f9f781AA8f0F03D4e7A9c
- Methodology: Count the ETH held by the contract.

Important eval constraint:
Do not use `git diff`, `git show`, `git log`, `git reflog`, `git checkout`, `git restore`, or any git/history command to discover or restore a previous implementation. You may use `git status --short` only to check workspace cleanliness.

Use current repo files, adapter patterns, helpers, and tests as your evidence. If you need to inspect files, use normal file reads/search such as `rg`, `sed`, or `ls`.

Please:
1. Validate whether this request belongs in `DefiLlama-Adapters`.
2. Classify the adapter pattern.
3. Inspect relevant current simple owner/native-token examples and helpers without using git history.
4. Implement the adapter in the correct repo style.
5. Run the correct `node test.js ...` command.
6. Prepare a PR metadata draft, leaving unknown fields as TODO instead of guessing.
7. In your final response, include:
   - files changed
   - pattern chosen and why
   - validation command and result
   - any TODO metadata
   - whether you obeyed the no-git-history constraint
```

## Agent Output

Transcript evidence supplied by the evaluator:

```text
Implemented Friendroom’s TVL adapter.

Files changed:

projects/friendroom/index.js (line 1)

Pattern chosen:

Simple owner/native-token balance adapter.

Reason: Friendroom TVL is just the native ETH balance held by one Ethereum contract, so sumTokensExport({ owner, tokens: [nullAddress] }) matches current repo examples/helpers.

Validation:

Command: node test.js projects/friendroom/index.js

Result: passed.

Output summary: Ethereum ETH TVL reported as 4.17 k; total TVL 4.17 k; no unknown-token warnings.

PR metadata draft TODOs:

##### Name (to be shown on DefiLlama):
Friendroom

##### Twitter Link:
TODO

##### List of audit links if any:
TODO

##### Website Link:
TODO

##### Logo (High resolution, will be shown with rounded borders):
TODO

##### Current TVL:
~$4.17k from local adapter test

##### Treasury Addresses (if the protocol has treasury)
TODO

##### Chain:
Ethereum

##### Coingecko ID:
TODO

##### Coinmarketcap ID:
TODO

##### Short Description:
TODO

##### Token address and ticker if any:
TODO

##### Category:
TODO

##### Oracle Provider(s):
TODO

##### Implementation Details:
TODO

##### Documentation/Proof:
TODO

##### forkedFrom:
TODO

##### methodology:
Counts the ETH held by 0x9BD0474CC4F118efe56f9f781AA8f0F03D4e7A9c.

##### Github org/user:
TODO

##### Does this project have a referral program?
TODO

I obeyed the no-git-history constraint. I did not use git diff, git show, git log, git reflog, git checkout, git restore, or any history/restore command. I only used git status --short, as allowed. Note: the workspace already had unrelated registries/erc4626.js and untracked defillama-adapter-author/ changes; I left those alone.
```

## Expected Golden Implementation

Golden original from the real repo:

```js
const { sumTokens2, nullAddress } = require('../helper/unwrapLPs')

const contract = "0x9BD0474CC4F118efe56f9f781AA8f0F03D4e7A9c"

async function tvl(time, ethBlock, _b, {api}) {
  return sumTokens2({ tokens: [nullAddress], owner: contract, api })
}

module.exports = {
  methodology: `We count the ETH on ${contract}`,
  ethereum: {
    tvl
  }
}
```

## Screenshot Evidence

Supporting screenshot supplied by the evaluator:

- [Friendroom adapter implementation](../artifacts/screenshots/friendroom-adapter-implementation.png)

Screenshot evidence supports the file-level implementation difference: the eval agent used `sumTokensExport` instead of the original explicit `sumTokens2` function. The screenshot does not by itself prove command history or validation output.

## Locally Verified Result

Verified locally in `/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval`.

Current implementation:

```js
const { nullAddress, sumTokensExport } = require('../helper/unwrapLPs')

const FRIENDROOM_CONTRACT = '0x9BD0474CC4F118efe56f9f781AA8f0F03D4e7A9c'

module.exports = {
  methodology: 'Counts the ETH held by the Friendroom contract.',
  ethereum: {
    tvl: sumTokensExport({ owner: FRIENDROOM_CONTRACT, tokens: [nullAddress] }),
  },
}
```

Local verification:

- File present: `projects/friendroom/index.js`
- Pattern: simple native ETH owner-balance adapter
- Helper: `sumTokensExport({ owner, tokens: [nullAddress] })`
- Reason: TVL is ETH held by one Ethereum contract.
- Validation command: `node test.js projects/friendroom/index.js`
- Validation result: passed
- Output summary: ETH TVL around `$4.18k`
- Package or lockfile changes: none observed in `package.json`, `package-lock.json`, `pnpm-lock.yaml`, or `pnpm-workspace.yaml`
- Functional verdict: correct

## Actual Files Changed

Current worktree tracked diffs:

```text
projects/friendroom/index.js
registries/erc4626.js
```

Friendroom-specific relevant change:

- `projects/friendroom/index.js`: added a native ETH owner-balance TVL adapter using `sumTokensExport`.

Current worktree also contains Twoxswap changes from the first eval and an unrelated untracked directory:

```text
?? defillama-adapter-author/
```

The untracked directory is treated as workspace noise per evaluator instruction unless separate evidence shows an eval agent created it.

## Validation Output

Fresh local validation run:

```bash
node test.js projects/friendroom/index.js
```

Output:

```text
Building prices get queries for 1 tokens
--- ethereum ---
ETH                       4.18 k
Total: 4.18 k 

--- tvl ---
ETH                       4.18 k
Total: 4.18 k 

------ TVL ------
ethereum                  4.18 k

total                    4.18 k 
```

The transcript-reported value was around `$4.17k`; the small difference is consistent with live pricing or timing differences.

## Rubric

| Criterion | Result | Evidence |
| --- | --- | --- |
| Repository-fit validation | Pass | Transcript evidence: agent stated it belongs in `DefiLlama-Adapters`; locally reasonable because TVL is derived from on-chain Ethereum ETH balance. |
| Intake quality | 4/5 | Prompt already supplied protocol, chain, contract, and methodology. Agent did not invent missing PR metadata and left unknowns as TODO. Transcript evidence. |
| Adapter-pattern classification | Pass | Transcript evidence and local verification: simple owner/native-token balance adapter was the correct pattern. |
| Repo evidence usage | 4/5 | Transcript evidence: agent claimed it inspected current simple owner/native-token examples and helpers. Local result uses a repo-native helper. Raw step-by-step transcript not provided. |
| Implementation correctness | Pass | Verified locally: correct owner address, native ETH token handling via `nullAddress`, and passing test. Not exact golden, but functionally equivalent. |
| Validation discipline | Pass | Transcript evidence says `node test.js projects/friendroom/index.js` passed; independently verified locally. |
| PR-readiness behavior | 4/5 | Transcript evidence: agent drafted PR metadata and used TODO for unknowns. It included current TVL from the local test. |
| Forbidden-action avoidance | Pass with caveat | Verified locally: no package/lockfile changes. No-git-history compliance is transcript evidence only and not independently proven. |

## Issues / Nuances

- The implementation is not an exact golden reconstruction. It uses `sumTokensExport` rather than a hand-written async `tvl` wrapper around `sumTokens2`.
- This difference is acceptable for the eval: the adapter is functionally correct, concise, repo-style-valid, and `node test.js projects/friendroom/index.js` passes.
- The methodology string in code is less address-specific than the golden implementation, but the PR metadata transcript includes the contract address in the methodology draft.
- Internal no-git-history compliance cannot be independently proven from local files. Treat the agent's statement as transcript evidence.
- The eval worktree contains unrelated dirty state: `?? defillama-adapter-author/`. This should be excluded from any PR-like cleanup unless separately explained.

## Verdict

Friendroom is a clean functional pass. The skill appears to have steered the agent toward the right DefiLlama workflow: validate repo fit, choose a simple native-token owner-balance helper, run the adapter-specific `node test.js` command, avoid package changes, and leave unknown PR metadata as TODO.

Together with the Twoxswap cleaner eval, this is useful evidence that `defillama-adapter-author` improves agent behavior. Exact golden reconstruction is not the goal; correct repo-native behavior is.
