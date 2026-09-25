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

The treasury is a gas sponsor. Its configured `grant_configs` decide which
callers qualify, and its configured allowance type and limits decide what the
sponsorship permits. A caller receiving the sponsorship those settings authorize
is the design working, not an attacker defeating it.

Fee-grant findings are in scope when a caller obtains sponsorship the treasury's
configuration does **not** authorize — for example, by bypassing or forging the
authz checks, obtaining a grant for an unauthorized grantee, changing the
configured allowance type or limits, or causing the treasury to sponsor message
types outside the configured allowance.

Repeated or renewed grants remain out of scope when the caller satisfies the
configured authorization on every issuance. A per-grant `spend_limit` is not a
lifetime cap, and aggregate use by many independently authorized callers is not
an authorization bypass. Consequences of an operator's own allowance choices
are likewise out of scope. The program covers failures to enforce the configured
policy, not the policy's intended operation.

## Severity

| Severity     | Description                                                                                                                                                                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CRITICAL** | Direct, permanent, irrecoverable theft or loss of funds held in or routed through covered contracts at meaningful scale. Complete bypass of account authentication where the proof of concept demonstrates actual movement of funds from a pre-existing victim account to an attacker-controlled address using only attacker-controlled keys. Permanent state corruption with no recovery path |
| **HIGH**     | Theft or freezing of funds affecting individual accounts. Authentication bypass with demonstrated exploitability against an existing account. Permanent disruption of core contract functionality                                                                                                                |
| **MEDIUM**   | Limited fund loss requiring specific preconditions. Privileged-party cooperation where the demonstrated impact uses authority the role already has (see Privileged Actor Policy). Temporary disruption recoverable by governance |
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
should be executed via standard transaction broadcast against that node. Show
inclusion in a block, the execution result, and the resulting state change or
security impact; broadcast acceptance alone is not sufficient. When transaction
failure is itself the claimed disruption, a nonzero execution result is
acceptable, but the proof must show the included failing transaction and its
observable impact on otherwise valid contract operations.

## Permissioned Chain Policy

XION mainnet operates with `code_upload_access: Nobody`. Uploading new contract
code requires governance approval.

An attack that depends on uploading attacker-controlled contract code to
mainnet is out of scope. A finding in one of the two scoped contracts is not
excluded solely because its proof of concept instantiates or interacts with
code already approved for mainnet, but the vulnerability must reside in and
affect a contract listed in Assets in Scope.

## Privileged Actor Policy

Findings are classified at **Medium at most** when the attack must begin with
control of a contract administrator, governance, or another privileged role —
or requires that holder to cooperate — and the demonstrated impact depends on
that holder acting self-destructively, outside normal operation, or in collusion
while using authority the role already has.

The cap does not apply when a flaw lets an attacker who starts without that
privilege obtain it or bypass its authorization check, or lets a legitimately
held limited role exercise authority that role was not granted. Those
findings are assessed by demonstrated impact. The program does not authorize
researchers to acquire or exercise production privileges they do not
legitimately control, or to test with production privileges they do control.

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

- Attacks requiring new attacker-controlled contract code to be uploaded to
  mainnet
- Denial of service requiring sustained attacker resource expenditure
  proportional to the harm caused
- Fee-grant issuance where the caller satisfies the configured authorization
  for that issuance and the resulting allowance preserves the configured type,
  limits, and message restrictions
- Governance attacks requiring a malicious proposal to pass
- Theoretical vulnerabilities without a working end-to-end proof of concept
- Attacks where the attacker's cost to execute exceeds the demonstrable harm to
  the protocol or its users
- Best practices, gas optimizations, missing events, and informational findings
