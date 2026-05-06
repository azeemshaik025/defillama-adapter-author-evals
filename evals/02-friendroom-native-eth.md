# defillama-tvl-adapter-author eval: Friendroom

## Eval summary

This eval used a known adapter as a control.

Friendroom already existed in `DefiLlama-Adapters`. We deleted its adapter file in a separate worktree, then asked an agent to add it back without git/history commands.

The result was functionally correct.

The agent used a simple native ETH owner-balance helper, ran `node test.js projects/friendroom/index.js`, and left unknown PR metadata as TODO.

Evidence:

- Functional result: Verified locally.
- Workflow claims: Transcript evidence.
- Code-change screenshot: Screenshot evidence.
- Internal no-git-history compliance: Transcript evidence only, not independently proven.

Exact golden reconstruction was not the goal.

The agent used `sumTokensExport({ owner, tokens: [nullAddress] })` instead of the original async `sumTokens2` wrapper. That is equivalent for this adapter.

## Eval fixture

Before the eval, this file was deleted from the eval worktree:

```text
projects/friendroom/index.js
```

Eval worktree:

```text
/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval
```

The prompt gave the agent the protocol facts: chain, contract address, and methodology.

The prompt also banned git/history commands used to discover or restore old code.

Allowed evidence was current repo files, helpers, examples, and tests.

This setup gives a fair comparison point without asking for an exact copy.

No-git-history compliance is transcript evidence only unless direct tool logs are available.

## Skill version and source

- Skill name during eval: `defillama-adapter-author`
- Final PR skill name: `defillama-tvl-adapter-author`
- Source repo: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters`
- Source repo HEAD after rename: `ddffda85f`
- Skill path during eval: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters/skills/defillama-adapter-author/SKILL.md`
- Final PR skill path: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters/skills/defillama-tvl-adapter-author/SKILL.md`
- Eval worktree: `/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval`
- Evidence note date: 2026-05-06
- Version: local PR skill, not a published/tagged release

Name note: this run used the original skill name in the prompt. The skill was renamed before PR review for clearer TVL scope.

## Prompt given to agent

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

## Agent output

Transcript evidence:

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

## Expected golden implementation

Original implementation from the real repo:

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

## Screenshot evidence

Screenshot supplied by the evaluator:

- [Friendroom adapter implementation](../artifacts/screenshots/friendroom-adapter-implementation.png)

The screenshot shows the adapter-file context and the implementation difference.

It shows `sumTokensExport` instead of the original explicit `sumTokens2` function.

It does not prove command history or validation output.

## Locally verified result

Checked locally in:

```text
/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval
```

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

Local checks:

- File present: `projects/friendroom/index.js`
- Pattern: simple native ETH owner-balance adapter
- Helper: `sumTokensExport({ owner, tokens: [nullAddress] })`
- Reason: TVL is ETH held by one Ethereum contract.
- Validation command: `node test.js projects/friendroom/index.js`
- Validation result: passed
- Output summary: ETH TVL around `$4.18k`
- Package or lockfile changes: none observed in `package.json`, `package-lock.json`, `pnpm-lock.yaml`, or `pnpm-workspace.yaml`
- Functional verdict: correct

## Actual files changed

Current tracked diffs:

```text
projects/friendroom/index.js
registries/erc4626.js
```

Friendroom change:

- `projects/friendroom/index.js`: added a native ETH owner-balance TVL adapter using `sumTokensExport`.

The worktree also contains Twoxswap changes from the first eval and one untracked directory:

```text
?? defillama-adapter-author/
```

We treat the untracked directory as workspace noise unless separate evidence shows an eval agent created it.

## Validation output

Fresh local validation:

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

The transcript value was around `$4.17k`.

The local run showed `$4.18k`. That small difference is expected for live pricing or timing.

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

## Issues and notes

- The implementation is not an exact copy of the original.
- It uses `sumTokensExport` instead of a hand-written async `tvl` wrapper around `sumTokens2`.
- This is acceptable here.
- The adapter is functionally correct, concise, follows repo style, and `node test.js projects/friendroom/index.js` passes.
- The code methodology is less address-specific than the original.
- The PR metadata transcript includes the contract address in its methodology draft.
- Internal no-git-history compliance cannot be independently proven from local files. Treat the agent's statement as transcript evidence.
- The eval worktree contains unrelated dirty state: `?? defillama-adapter-author/`.
- That directory should stay out of PR cleanup unless separately explained.

## Verdict

Friendroom passed.

The agent chose a simple native-token owner-balance helper. It ran the right test. It did not touch package files. It left unknown PR fields as TODO.

This is the behavior we wanted to test.
