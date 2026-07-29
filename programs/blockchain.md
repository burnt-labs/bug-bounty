# Bug Bounty — Blockchain / DLT

Covers the XION chain node and its supporting protocol infrastructure — the
custom Cosmos SDK modules, the abstract account system, and the ZK proof
verification bindings.

Read the [program terms](../README.md) first — reporting channels, reward
policy, KYC requirement, disclosure rules, and safe harbour apply to this
program.

## Assets in Scope

| Repository | Covers |
| ---------- | ------ |
| [`burnt-labs/xion`](https://github.com/burnt-labs/xion) | XION chain node and all custom Cosmos SDK modules |
| [`burnt-labs/abstract-account`](https://github.com/burnt-labs/abstract-account) | Abstract account module and authenticator contract infrastructure |
| [`burnt-labs/barretenberg-go`](https://github.com/burnt-labs/barretenberg-go) | Go bindings and proof verification wrappers for the Barretenberg ZK proving library |

Scope applies to the current mainnet release. Findings affecting only deprecated
or end-of-life versions, or already remediated in the currently deployed mainnet
version, are not eligible regardless of whether the fix was publicly announced.
Verify exploitability against the currently deployed version before submitting.

## Severity

| Severity     | Description                                                                                                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CRITICAL** | Direct, permanent, irrecoverable theft or loss of user funds at protocol scale. Unauthorized minting. Chain halt or consensus failure requiring a hard fork to resolve. Complete bypass of abstract account authentication enabling arbitrary transaction authorization |
| **HIGH**     | Theft or freezing of user funds affecting individual accounts. Significant authentication bypass with demonstrated exploitability                                                                                                              |
| **MEDIUM**   | Limited fund loss or temporary disruption requiring specific preconditions. Attacks requiring privileged-party cooperation. Partial authentication bypass requiring secondary conditions                                                        |
| **LOW**      | Valid, reproducible code-level issue with no direct risk to funds or chain safety, representing a meaningful hardening opportunity. Must include a specific code reference                                                                      |

Only **High** and **Critical** are reward eligible.

### Authentication Impact Scope

Authentication weaknesses whose impact is limited to accounts created after the
attack is established — and which cannot affect the funds, state, or
authentication of any account funded and operational before the attack began —
are capped at **Medium**, regardless of the authentication mechanism involved.

A High or Critical authentication finding must demonstrate unauthorized impact
on a **pre-existing funded account**.

## Proof of Concept

**An end-to-end proof of concept is required.**

Unit tests using `setupKeeper(t)` or similar harnesses bypass transaction
encoding, routing, and the ante handler chain, and do not demonstrate on-chain
exploitability on their own.

The proof of concept should run against a **locally running XION node configured
with mainnet parameters** — the same setup used by the end-to-end test suite in
[`burnt-labs/xion`](https://github.com/burnt-labs/xion), with the XION ante
handler chain, module set, and governance configuration matching mainnet. The
attack should be executed via standard transaction broadcast (`BroadcastTxSync`
or equivalent) against that node. Simulated environments that model chain state
without running a full node do not demonstrate exploitability.

For findings in `barretenberg-go`, the relevant boundary is the **binding
layer** — how proofs, verification keys, and public inputs cross between Go and
the underlying C library. Include the exact proof, verification key, and public
input bytes that produce the incorrect result, along with a runnable Go test.

## Permissioned Chain Policy

XION mainnet operates with `code_upload_access: Nobody`. Contract deployment
requires a governance proposal. **This is a fundamental architectural
constraint, not a bypass target.**

Any attack vector requiring an attacker to deploy a malicious contract on
mainnet is out of scope, regardless of technical validity. This includes
amplification attacks via attacker-deployed contracts, exploit chains initiated
from attacker-deployed contracts, and any scenario beginning with "an attacker
deploys a contract that...".

## Privileged Actor Policy

Attacks requiring a privileged party — governance, a module authority, or a
validator — to take self-destructive or colluding action are classified at
**Medium at most**, regardless of downstream impact. This includes validators
supplying unusual inputs, extreme timestamps, delayed responses, or off-spec
data to consensus rounds. The threat model assumes privileged actors operate
within the specified protocol parameters.

## Out of Scope

**Assets**

- Smart contracts — see [Core Protocol Contracts](contracts.md)
- Frontend applications — see [Websites and Applications](websites.md)
- Third-party infrastructure, RPC providers, and external dependencies
- Public blockchain RPC, REST, gRPC, and Tendermint RPC endpoints. These expose
  blockchain state by design and are operated by validators and node operators
  as a public service
- Upstream dependencies. Vulnerabilities in CosmWasm, the Cosmos SDK, IBC, or
  the Barretenberg C library are not eligible here; only code originating in the
  in-scope repositories is covered

**Vulnerability classes**

- Attacks requiring malicious contract deployment on mainnet
- Denial of service of any form, including single-transaction resource
  exhaustion, node crashes, and chain halts recoverable via a software patch,
  coordinated validator restart, or governance parameter update. Chain halts
  requiring a hard fork to resolve remain in scope under Critical
- Governance attacks requiring a malicious proposal to pass
- Theoretical vulnerabilities without a working end-to-end proof of concept
- Attacks where the attacker's cost to execute exceeds the demonstrable harm to
  the protocol or its users
- Best practices, gas optimizations, missing events, and informational findings
