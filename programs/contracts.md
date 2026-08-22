# Bug Bounty — Core Protocol Contracts

Covers the core smart contracts deployed on the XION blockchain. These are
governance-deployed and form the foundation of XION's account abstraction and
fee infrastructure.

Read the [program terms](../README.md) first — reporting channels, reward
policy, KYC requirement, disclosure rules, and safe harbour apply to this
program.

## Assets in Scope

| Contract | Source                                                                                 |
| -------- | -------------------------------------------------------------------------------------- |
| Account  | [`burnt-labs/account-contract`](https://github.com/burnt-labs/account-contract)         |
| Treasury | [`burnt-labs/treasury-contract`](https://github.com/burnt-labs/treasury-contract)       |

**Scope is limited exclusively to the two contracts above.**

Scope applies to contracts deployed on the current mainnet. Findings affecting
only deprecated deployments, or already remediated in the currently deployed
bytecode, are not eligible regardless of whether the fix was publicly announced.
Verify exploitability against the current deployed contract version before
submitting.

### Treasury Fee Grant Scope

Fee grant issuance findings are in scope where an **unprivileged caller can
extract value from the treasury's XION balance without effective bound**.

A grant is treated as bounded — and therefore out of scope — only when its
allowance and expiration **cannot be reset, refreshed, revoked and reissued, or
otherwise renewed by an unprivileged caller**. Where a configured spend cap can
be renewed, the cap does not bound total extraction in practice, and the finding
is in scope.

Findings against genuinely bounded grant operations are not eligible. That
design intentionally delegates authorization to the calling application layer,
and a bounded grant behaving as specified is not a vulnerability.

## Severity

| Severity     | Description                                                                                                                                                                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CRITICAL** | Direct, permanent, irrecoverable theft or loss of funds held in or routed through covered contracts at meaningful scale. Complete bypass of account authentication where the proof of concept demonstrates actual movement of funds from a pre-existing victim account to an attacker-controlled address using only attacker-controlled keys. Permanent state corruption with no recovery path |
| **HIGH**     | Theft or freezing of funds affecting individual accounts. Authentication bypass with demonstrated exploitability against an existing account. Permanent disruption of core contract functionality                                                                                                                |
| **MEDIUM**   | Limited fund loss requiring specific preconditions. Attacks requiring privileged-party cooperation. Temporary disruption recoverable by governance                                                                                                                                                              |
| **LOW**      | Valid, reproducible code-level issue with no direct risk to funds, representing a meaningful hardening opportunity. Must include a specific code reference                                                                                                                                                       |

Only **High** and **Critical** are reward eligible.

## Proof of Concept

**An end-to-end proof of concept is required.**

Tests that mock contract state or bypass CosmWasm message routing — including
`cw-multi-test` environments and harnesses that stub the bank, staking, or IBC
modules — do not demonstrate exploitability on their own.

The proof of concept should run against a **locally running XION node configured
with mainnet parameters**, using the governance-deployed contract bytecode, the
XION ante handler chain, and module configuration matching mainnet. The attack
should be executed via standard transaction broadcast against that node.

## Permissioned Chain Policy

XION mainnet operates with `code_upload_access: Nobody`. New contracts require
governance approval to deploy.

**Any attack vector requiring an attacker to deploy a malicious contract on
mainnet is out of scope, regardless of technical validity.** A finding must be
exploitable using only contracts already deployed on mainnet.

## Privileged Actor Policy

Attacks requiring a contract admin, governance, or another privileged party to
take self-destructive or colluding action are classified at **Medium at most**,
regardless of downstream impact. The threat model assumes privileged actors
behave according to their role.

## Out of Scope

**Assets**

- Any contract not listed in Assets in Scope, including `asset`,
  `marketplace`, and `user_map`
- Example and demo contracts
- Third-party contracts deployed on XION by external teams
- Chain node modules — see [Blockchain / DLT](blockchain.md)
- Applications and the client SDK — see [Applications and SDKs](applications.md)
- Upstream dependencies. Vulnerabilities in CosmWasm or the Cosmos SDK are not
  eligible here; only code originating in the in-scope contracts is covered

**Vulnerability classes**

- Attacks requiring malicious contract deployment on mainnet
- Denial of service requiring sustained attacker resource expenditure
  proportional to the harm caused
- Fee grant operations that are bounded as defined above
- Governance attacks requiring a malicious proposal to pass
- Theoretical vulnerabilities without a working end-to-end proof of concept
- Attacks where the attacker's cost to execute exceeds the demonstrable harm to
  the protocol or its users
- Best practices, gas optimizations, missing events, and informational findings
