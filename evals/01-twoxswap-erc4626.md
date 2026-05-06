# defillama-tvl-adapter-author eval: Twoxswap

## Eval summary

This eval used a known adapter as a control.

Twoxswap already existed in `DefiLlama-Adapters`. We removed its registry entry in a separate worktree, then asked an agent to add it back without git/history commands.

The result was functionally correct.

The agent used the ERC4626 registry, ran `node test.js twoxswap`, and left unknown PR metadata as TODO.

Evidence:

- Functional result: Verified locally.
- Workflow claims: Transcript evidence.
- Code-change screenshots: Screenshot evidence.
- Internal no-git-history compliance: Transcript evidence only, not independently proven.

Exact golden reconstruction was not the goal.

The new entry is lower in the registry and uses `ERC4626` instead of `ERC-4626` in the methodology. That difference does not affect function.

## Eval fixture

Before the eval, the Twoxswap entry was removed from `registries/erc4626.js` in this worktree:

```text
/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval
```

The prompt gave the agent the protocol facts: chain, vault address, and methodology.

The prompt also banned git/history commands used to discover or restore old code.

Allowed evidence was current repo files, helpers, registries, examples, and tests.

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

I want to add Twoxswap to DefiLlama TVL.

Twoxswap has an ERC-4626 vault on Ethereum. TVL should count the total USDC deposited in the X2Pool ERC-4626 vault, measured via totalAssets().

Known details:
- Protocol: Twoxswap / 2xSwap
- Chain: Ethereum
- Vault address: 0x2a315Fef86916B30905086C85A9cB55E5DCD7ED3
- Methodology: TVL is the total USDC deposited in the X2Pool ERC-4626 vault, measured via totalAssets().

Important eval constraint:
Do not use `git diff`, `git show`, `git log`, `git reflog`, `git checkout`, `git restore`, or any git/history command to discover or restore a previous implementation. You may use `git status --short` only to check workspace cleanliness.

Use current repo files, adapter patterns, helpers, registries, and tests as your evidence. If you need to inspect files, use normal file reads/search such as `rg`, `sed`, or `ls`.

Please:
1. Validate whether this request belongs in `DefiLlama-Adapters`.
2. Classify the adapter pattern.
3. Inspect the relevant current registry/helper/example patterns without using git history.
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
Implemented Twoxswap as a registry-backed ERC-4626 TVL adapter.

Changed

registries/erc4626.js (line 267): added twoxswap with the Ethereum X2Pool vault and methodology.

Fit And Pattern

Belongs in DefiLlama-Adapters: TVL is computed from on-chain Ethereum contract data.

Pattern: registries/erc4626.js, because this is a plain ERC-4626 vault list where the helper reads asset() and totalAssets().

Validation

Ran: node test.js twoxswap

Result: passed.

Output priced the vault as USDC with total TVL around $517.27.

PR Metadata Draft

Name: Twoxswap / 2xSwap
Twitter Link: TODO
List of audit links if any: TODO
Website Link: TODO
Logo: TODO
Current TVL: ~$517.27 from local adapter test
Treasury Addresses: TODO
Chain: Ethereum
Coingecko ID: TODO
Coinmarketcap ID: TODO
Short Description: TODO
Token address and ticker if any: TODO
Category: TODO
Oracle Provider(s): TODO
Implementation Details: TODO
Documentation/Proof: TODO
forkedFrom: TODO
methodology: TVL is the total USDC deposited in the X2Pool ERC4626 vault, measured via totalAssets().
Github org/user: TODO
Does this project have a referral program?: TODO

I obeyed the no-git-history constraint: I did not run git diff, git show, git log, git reflog, git checkout, or git restore. I only used git status --short, which also showed pre-existing unrelated workspace changes.
```

## Expected golden implementation

Original implementation from the real repo:

```js
'twoxswap': {
  methodology: 'TVL is the total USDC deposited in the X2Pool ERC-4626 vault, measured via totalAssets().',
  ethereum: ['0x2a315Fef86916B30905086C85A9cB55E5DCD7ED3']
},
```

## Screenshot evidence

Screenshots supplied by the evaluator:

- [Twoxswap registry context / removal fixture](../artifacts/screenshots/twoxswap-registry-context.png)
- [Twoxswap registry addition](../artifacts/screenshots/twoxswap-registry-addition.png)

The screenshots show the removed and added registry context.

They do not prove command history or validation output.

## Locally verified result

Checked locally in:

```text
/Users/azeemshaik/work/opensource/DefiLlama-Adapters-skill-eval
```

Current relevant implementation:

```js
'twoxswap': {
  methodology: "TVL is the total USDC deposited in the X2Pool ERC4626 vault, measured via totalAssets().",
  ethereum: ['0x2a315Fef86916B30905086C85A9cB55E5DCD7ED3'],
}
```

Local checks:

- File present: `registries/erc4626.js`
- Pattern: registry-backed ERC4626 adapter
- Reason: the ERC4626 registry helper builds exports for vaults and reads vault asset / totalAssets data.
- Validation command: `node test.js twoxswap`
- Validation result: passed
- Output summary: USDC TVL around `$517.24`
- Package or lockfile changes: none observed in `package.json`, `package-lock.json`, `pnpm-lock.yaml`, or `pnpm-workspace.yaml`
- Functional verdict: correct

## Actual files changed

Current tracked diffs:

```text
projects/friendroom/index.js
registries/erc4626.js
```

Twoxswap change:

- `registries/erc4626.js`: added `twoxswap` to the ERC4626 registry.

The worktree also contains Friendroom changes from the second eval and one untracked directory:

```text
?? defillama-adapter-author/
```

We treat the untracked directory as workspace noise unless separate evidence shows an eval agent created it.

## Validation output

Fresh local validation:

```bash
node test.js twoxswap
```

Output:

```text
Loaded module twoxswap from registry
Building prices get queries for 1 tokens
--- ethereum ---
USDC                      517.24
Total: 517.24 

--- tvl ---
USDC                      517.00
Total: 517.24 

------ TVL ------
ethereum                  517.00

total                    517.24 
```

The transcript value was around `$517.27`.

The local run showed `$517.24`. That small difference is expected for live pricing or timing.

## Rubric

| Criterion | Result | Evidence |
| --- | --- | --- |
| Repository-fit validation | Pass | Transcript evidence: agent stated it belongs in `DefiLlama-Adapters`; locally reasonable because TVL is derived from on-chain Ethereum ERC4626 data. |
| Intake quality | 4/5 | Prompt already supplied protocol, chain, vault, and methodology. Agent did not invent missing PR metadata and left unknowns as TODO. Transcript evidence. |
| Adapter-pattern classification | Pass | Transcript evidence and local verification: registry-backed ERC4626 was the correct pattern. |
| Repo evidence usage | 4/5 | Transcript evidence: agent claimed it inspected current registry/helper/example patterns. Local result matches repo style. Raw step-by-step transcript not provided. |
| Implementation correctness | Pass | Verified locally: correct vault address, ERC4626 registry shape, and passing test. Exact placement/text differs from golden but is functionally valid. |
| Validation discipline | Pass | Transcript evidence says `node test.js twoxswap` passed; independently verified locally. |
| PR-readiness behavior | 4/5 | Transcript evidence: agent drafted PR metadata and used TODO for unknowns. It did not overclaim missing website/social/audit/category metadata. |
| Forbidden-action avoidance | Pass with caveat | Verified locally: no package/lockfile changes. No-git-history compliance is transcript evidence only and not independently proven. |

## Issues and notes

- The implementation is not an exact copy of the original.
- It adds the entry lower in `registries/erc4626.js`.
- It uses `ERC4626` instead of `ERC-4626` in the methodology text.
- These differences are acceptable here.
- The adapter is functionally correct, follows repo style, and `node test.js twoxswap` passes.
- Internal no-git-history compliance cannot be independently proven from local files. Treat the agent's statement as transcript evidence.
- The eval worktree contains unrelated dirty state: `?? defillama-adapter-author/`.
- That directory should stay out of PR cleanup unless separately explained.

## Verdict

Twoxswap passed.

The agent chose the ERC4626 registry, not a custom adapter. It ran the right test. It did not touch package files. It left unknown PR fields as TODO.

This is the behavior we wanted to test.
