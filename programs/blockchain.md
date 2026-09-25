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
| [`burnt-labs/wasmd`](https://github.com/burnt-labs/wasmd) | XION's fork of `CosmWasm/wasmd` — the contract execution module. **Burnt Labs' patches only** |
| [`burnt-labs/tokenfactory`](https://github.com/burnt-labs/tokenfactory) | XION's fork of `strangelove-ventures/tokenfactory`. **Burnt Labs' patches only** |

Scope applies to the current mainnet release. Findings affecting only deprecated
or end-of-life versions, or already remediated in the currently deployed mainnet
version, are not eligible regardless of whether the fix was publicly announced.
Verify exploitability against the currently deployed version before submitting.

### Fork Scope

The last two repositories are forks the chain node builds against through
`replace` directives in [`burnt-labs/xion`](https://github.com/burnt-labs/xion)'s
`go.mod`. They ship on mainnet under `-xion.N` version tags.

**Only the delta between the fork and its upstream base is in scope.** When the
fork tag extends an upstream release tag, remove the `-xion.N` suffix to identify
the base. When no corresponding upstream tag exists, use the exact upstream
commit named in the mapping table below as the base. A finding that reproduces
on the unmodified upstream base belongs to the upstream project, not to this
program, and is not eligible here regardless of its impact on XION.

For the fork versions in the current mainnet release, the bases are:

| Fork tag | Upstream base |
| -------- | ------------- |
| `burnt-labs/wasmd@v0.61.14-xion.3` | `CosmWasm/wasmd@v0.61.14` |
| `burnt-labs/tokenfactory@v0.53.4-xion.3` | `strangelove-ventures/tokenfactory@dacc99329b029248b965dcb025c869bc5cd7296d` (`v0.50.7-wasmvm2`) |

Before a new mainnet fork tag replaces one of these mappings, this table must be
updated in the same change to name that fork tag's upstream base tag or exact
commit.

## Severity

| Severity     | Description                                                                                                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CRITICAL** | Direct, permanent, irrecoverable theft or loss of user funds at protocol scale. Unauthorized minting. Chain halt or consensus failure requiring a hard fork to resolve. Complete bypass of abstract account authentication enabling arbitrary transaction authorization |
| **HIGH**     | Theft or freezing of user funds affecting individual accounts. Significant authentication bypass with demonstrated exploitability                                                                                                              |
| **MEDIUM**   | Limited fund loss or temporary disruption requiring specific preconditions. Privileged-party cooperation where the demonstrated impact uses authority the role already has (see Privileged Actor Policy). Partial authentication bypass requiring secondary conditions |
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
or equivalent) against that node. Broadcast acceptance alone is not sufficient:
show inclusion in a block, the successful execution result, and the resulting
state change or security impact. For chain-halt or consensus-failure findings,
instead show the triggering transaction or input sequence, the height or round
at which progress stops or diverges, and the observed halt or failure condition;
block inclusion and successful execution are not required when the failure
prevents them. Simulated environments that model chain state without running a
full node do not demonstrate exploitability.

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

Findings are classified at **Medium at most** when the attack must begin with
control of governance, a module authority, validator or operator credentials,
or another privileged role — or requires that holder to cooperate — and the
demonstrated impact depends on that holder acting self-destructively, outside
normal operation, or in collusion while using authority the role already has.
This includes validators deliberately supplying unusual inputs, extreme
timestamps, delayed responses, or off-spec data to consensus rounds.

The cap does not apply when a flaw lets an attacker who starts without that
privilege obtain it or bypass its authorization check, or lets a legitimately
held limited role exercise authority that role was not granted. Those
findings are assessed by demonstrated impact. The program does not authorize
researchers to acquire or exercise production privileges they do not
legitimately control, or to test with production privileges they do control.

## Out of Scope

**Assets**

- Smart contracts — see [Core Protocol Contracts](contracts.md)
- Applications and the client SDK — see [Applications and SDKs](applications.md)
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
