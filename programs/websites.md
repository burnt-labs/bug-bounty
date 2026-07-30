# Bug Bounty — Websites and Applications

Covers the production web properties operated by Burnt Labs where a
vulnerability could result in user harm, credential compromise, or unauthorized
transaction execution.

Read the [program terms](../README.md) first — reporting channels, reward
policy, KYC requirement, disclosure rules, and safe harbour apply to this
program.

> **Reporting channel.** Most repositories behind these properties are private,
> so GitHub private vulnerability reporting is not available for them. Report
> findings in this program to [security@burnt.com](mailto:security@burnt.com).
> Eligibility follows the asset, not the channel — reporting by email does not
> affect it.

## Assets in Scope

Scope is defined by **hostname**, not by repository. A property is in scope only
if its hostname appears below.

| Hostname | Property |
| -------- | -------- |
| `app.burnt.com` | XION web app — staking, governance, and smart account transactions |
| `auth.burnt.com` | Authentication |
| `auth-beta.burnt.com` | Authentication (beta) |
| `settings.burnt.com` | Account settings |
| `explorer.burnt.com` | Block explorer — see note below |
| `explorer.mainnet.burnt.com` | Block explorer — see note below |
| `explorer.xion.burnt.com` | Block explorer — see note below |

**Explorer note.** The block explorer is derived from an upstream open source
project. Only defects originating in Burnt Labs' own code are in scope.
Vulnerabilities present in the upstream project should be reported to that
project.

## Severity

| Severity     | Description                                                                                                                                                                                                                                                                                             |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CRITICAL** | Full account takeover or unauthorized transaction execution that does not require the victim to approve or confirm a security-sensitive action. The attack must succeed through passive exploitation — page load, background request — or be embedded in what appears to the victim as a routine interaction |
| **HIGH**     | Account or session compromise requiring limited, non-suspicious user interaction such as visiting a link or loading a page. Cross-site scripting with demonstrated ability to initiate or manipulate transactions. Significant data exposure affecting individual users                                    |
| **MEDIUM**   | Attacks requiring the victim to take a meaningful, security-relevant action — clicking through a confirmation, explicitly granting access, or following multi-step instructions. Limited data exposure or access control bypass requiring specific preconditions. CSRF with demonstrated impact on account state |
| **LOW**      | Valid security issue with no direct risk to accounts or user data, representing a meaningful hardening opportunity                                                                                                                                                                                       |

Only **High** and **Critical** are reward eligible.

Reports where the victim must be tricked into explicitly granting access,
authorizing a transaction, or clicking through a confirmation are **not**
Critical.

## Proof of Concept

A proof of concept demonstrating the vulnerability is required. **Do not
demonstrate it against production** — testing production disqualifies the
report. Run the property locally or against your own deployment.

Screenshots or a video walkthrough showing end-to-end exploitation are expected
for High and Critical reports. Reports consisting only of automated scanner
output, without demonstrated exploitability, are not rewarded.

## Out of Scope

**Assets**

- Any hostname not listed above, including testnet and staging hosts
  (`*.testnet.burnt.com` and equivalents), the developer portal, admin
  interfaces, faucets, marketing sites, and internal tooling
- Decommissioned properties that are no longer in use, including the staking
  interface at `staking.burnt.com` and its variants
- Public blockchain RPC, REST, gRPC, and Tendermint RPC endpoints
- Backend APIs and services. These are not currently covered by any program
- Third-party services and infrastructure
- Upstream open source code in the block explorer

**Vulnerability classes**

- Clickjacking. Transaction signing provides a second confirmation layer that
  mitigates the attack surface
- Open redirects after authentication that do not leak tokens or credentials
- Self-XSS requiring the attacker to execute code in their own browser session
- Issues requiring physical access to the victim's device
- Social engineering
- Denial of service
- Missing security headers where no exploitable impact is demonstrated
- Theoretical vulnerabilities without a demonstrated attack path and measurable
  user impact
- Best practices and informational findings
