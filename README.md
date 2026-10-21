# Burnt Labs Bug Bounty

Burnt Labs runs its bug bounty program directly. We do not use a third-party
bounty platform.

**The bug bounty program is currently paused.** We welcome submissions through
the channels below, but we will not respond to them and will not pay rewards.

This repository is the canonical source for program terms. Its commit history is
the record of what the terms said on any given date — if the terms change while
your report is open, the version in effect when you submitted is the one that
applies.

## Programs

| Program | Covers |
| ------- | ------ |
| [Blockchain / DLT](programs/blockchain.md) | XION chain node, custom Cosmos SDK modules, the forks the node builds against, abstract account system, ZK proof verification bindings |
| [Core Protocol Contracts](programs/contracts.md) | The account and treasury contracts |
| [Applications and SDKs](programs/applications.md) | Production applications operated by Burnt Labs, and the `xion.js` client SDK |
| [Websites](programs/websites.md) | Informational and marketing web properties, at a lower tier |

Each program defines its own assets, proof of concept requirements, and
exclusions. The terms below apply to all four.

## How to Report

Every program lists its assets explicitly. If an asset is not on a list, it is
not in scope and testing it is not authorized — there is no category, pattern,
or family of hostnames that confers scope on something we have not named. When
in doubt, do not test.

A program may pair a scoped production asset with an explicitly named
non-production testing endpoint. That endpoint authorizes testing only; it is not
a separate asset. Submit the report against the paired production
asset and explain why the demonstrated issue applies there.

There are two channels. **Please choose one.**

|         | GitHub private vulnerability reporting                            | Email                                           |
| ------- | ----------------------------------------------------------------- | ----------------------------------------------- |
| How     | **Security → Report a vulnerability** on the affected repository  | [security@burnt.com](mailto:security@burnt.com)  |
| Use for | Findings in a public repository named in a program                 | Everything else                                  |

Private vulnerability reporting is enabled on our public repositories and opens
a private advisory only Burnt Labs can see. Prefer it where it is available.

Use email for everything else. Some in-scope assets — the web properties in
particular — are built from private repositories where GitHub private
vulnerability reporting cannot be enabled. **Reporting those by email does not
affect their scope.**

**Do not open a public GitHub issue for a security vulnerability.** Public
disclosure before a patch is available increases the harm to users.

## Rewards

**No rewards are paid while the program is paused.** Submitting a report does
not entitle you to a bounty or any other payment.

## Privileged Access

Pre-existing privileged access is not itself an exploit primitive. Where a
program caps findings that require a privileged actor, the cap applies when the
attack must begin with control of governance, an administrator or module
authority, validator or operator credentials, or another privileged role — or
requires that holder to cooperate — and the demonstrated impact depends on that
holder acting self-destructively, outside normal operation, or in collusion
while using authority the role already has.

That cap does **not** apply when a flaw lets an attacker who starts without the
privilege obtain it or bypass its authorization check, or lets a legitimately
held limited role exercise authority that role was not granted. Those
findings are assessed on their demonstrated impact under the applicable
program's severity definitions.

Nothing in this program authorizes a researcher to acquire, borrow, compromise,
or exercise production credentials or privileges they do not legitimately
control. Reproduce privileged-role findings only with test roles and keys in a
local environment or infrastructure you control. Legitimately holding a
production role does not authorize testing with it against production.

## Severity

| Severity     | Description                                                                  |
| ------------ | ---------------------------------------------------------------------------- |
| **CRITICAL** | Immediate threat to critical systems — funds at risk, network compromise      |
| **HIGH**     | Significant impact on major functionality or security controls               |
| **MEDIUM**   | Impacts minor features or exposes non-sensitive data                          |
| **LOW**      | Minimal impact or informational                                               |

Each program narrows these definitions for its own assets. Where they differ,
the program document governs.

## Proof of Concept

Every report needs a proof of concept. Severity is assessed on demonstrated
impact under real-world constraints, not theoretical worst-case scenarios, and a
finding we cannot reproduce is difficult to act on.

Each program states what a sufficient proof of concept looks like for its
assets. In general, a report that only describes a suspected weakness — without
inputs, steps, or code that exhibit it — is not actionable.

## Responsible Disclosure

- Do not exploit a vulnerability beyond what is necessary to confirm it exists
- **Do not test against production systems.** This includes XION mainnet and live
  production web properties. Testing production disqualifies the report
- Use only testing endpoints explicitly named by the applicable program, a local
  environment, or infrastructure you control. Other XION testnets, staging,
  preview, and development deployments are not authorized by implication
- Do not access, modify, or exfiltrate user data
- Do not disrupt or degrade our networks, data, or services
- Do not disclose publicly before a fix is confirmed and deployed
- Allow us reasonable time to address the issue

## Response

While the program is paused we do not respond to submissions, and we make no
commitment to any acknowledgment, triage, or disclosure timeline.

## Safe Harbor

Burnt Labs will not pursue legal action against researchers who report
vulnerabilities in good faith under this policy, do not exploit beyond what is
necessary to confirm the finding, do not access or disclose user data, and do
not disrupt production systems.

**Naming a production asset establishes eligibility, not permission to test that
production system.** Authorization to actively test extends only to testing
endpoints explicitly named by the applicable program, local environments, and
infrastructure you control. Nothing else is authorized by implication. If you
are unsure whether something is covered, do not test it.

Reporting a vulnerability you encountered incidentally is always welcome, and
carries no expectation that you were authorized to be there.

## Recognition

If we publish an advisory for a finding, we may credit the researcher who
reported it.
