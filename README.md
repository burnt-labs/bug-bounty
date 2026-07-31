# Burnt Labs Bug Bounty

Burnt Labs runs its bug bounty program directly. We do not use a third-party
bounty platform, and reports are triaged by the engineers who maintain the
affected code.

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

**Reward eligibility follows the asset, not the channel.** An asset explicitly
named in one of the programs above is eligible however you report it. An asset
named in none of them is not eligible, however you report it — we still want the
report, and we will investigate and fix genuine issues.

Every program lists its assets explicitly. If an asset is not on a list, it is
not in scope and testing it is not authorized — there is no category, pattern,
or family of hostnames that confers scope on something we have not named. When
in doubt, ask at [security@burnt.com](mailto:security@burnt.com) **before**
testing.

A program may pair a scoped production asset with an explicitly named
non-production testing endpoint. That endpoint authorizes testing only; it is not
a separate reward-eligible asset. Submit the report against the paired production
asset and explain why the demonstrated issue applies there.

There are two channels. **Please choose one.**

|         | GitHub private vulnerability reporting                            | Email                                           |
| ------- | ----------------------------------------------------------------- | ----------------------------------------------- |
| How     | **Security → Report a vulnerability** on the affected repository  | [security@burnt.com](mailto:security@burnt.com)  |
| Use for | Findings in a public repository named in a program                 | Everything else                                  |

Private vulnerability reporting is enabled on our public repositories and opens
a private advisory only Burnt Labs can see. Prefer it where it is available: the
fix is developed against the report, and you are credited on the published
advisory and in any CVE we request.

Use email for everything else. Some in-scope assets — the web properties in
particular — are built from private repositories where GitHub private
vulnerability reporting cannot be enabled. **Reporting those by email does not
affect their eligibility.**

**Do not open a public GitHub issue for a security vulnerability.** Public
disclosure before a patch is available increases the harm to users.

## Rewards

**We do not publish reward amounts.** Bounties are determined during the
disclosure process, after severity has been assessed against demonstrated impact.

- Bounties are awarded for findings assessed as **High** or **Critical**. Medium
  and Low findings are valued and will be fixed, but are not rewarded.
- Only production assets explicitly named in a program above are eligible.
  Named testing endpoints authorize safe reproduction but are not separately
  reward eligible.
- **Testing against production disqualifies the report from any bounty.**
- Collecting a bounty requires completing a **KYC process**. We cannot pay
  reporters in sanctioned jurisdictions.
- Where several reports describe the same underlying issue, the first complete
  report with a working proof of concept is the one considered.

Severity is assessed by Burnt Labs against the definitions below and the
program-specific definitions in each document. We assess reports as submitted;
we do not reclassify a report to a different severity on a reporter's behalf.

## Severity

| Severity     | Description                                                                  | Reward eligible |
| ------------ | ---------------------------------------------------------------------------- | --------------- |
| **CRITICAL** | Immediate threat to critical systems — funds at risk, network compromise      | Yes             |
| **HIGH**     | Significant impact on major functionality or security controls               | Yes             |
| **MEDIUM**   | Impacts minor features or exposes non-sensitive data                          | No              |
| **LOW**      | Minimal impact or informational                                               | No              |

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

## Response Process

1. **Acknowledgment** — within **5 business days**
2. **Triage** — a triage decision within **14 days**
3. **Investigation** — we confirm the finding and assess severity
4. **Fix development** — developed and tested privately
5. **Coordination** — for critical issues we coordinate with affected parties and
   upstream projects through non-public channels before disclosure
6. **Community notification** — we notify the community that a security release
   is coming, so users, validators, and integrators can prepare
7. **Public disclosure** — after the fix is deployed we publish an advisory with
   details and credit

Active exploitation, or confirmed attacker awareness of an unpatched
vulnerability, escalates the issue to Critical handling regardless of its
original classification.

Where a fix requires a network upgrade, additional time may be needed to raise a
governance proposal and complete the upgrade.

## Safe Harbor

Burnt Labs will not pursue legal action against researchers who report
vulnerabilities in good faith under this policy, do not exploit beyond what is
necessary to confirm the finding, do not access or disclose user data, and do
not disrupt production systems.

**Naming a production asset establishes eligibility, not permission to test that
production system.** Authorization to actively test extends only to testing
endpoints explicitly named by the applicable program, local environments, and
infrastructure you control. Nothing else is authorized by implication. If you
are unsure whether something is covered, ask before you test it.

Reporting a vulnerability you encountered incidentally is always welcome, and
carries no expectation that you were authorized to be there.

## Recognition

We credit researchers on published advisories. Where a finding results in a
GitHub security advisory, you are named in its credits and in any CVE we request
for it.
