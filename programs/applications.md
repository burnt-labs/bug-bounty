# Bug Bounty — Applications and SDKs

Covers the production applications operated by Burnt Labs, and the client SDK
third-party developers build against, where a vulnerability could result in user
harm, credential compromise, or unauthorized transaction execution.

Informational and marketing web properties are covered separately, at a lower
tier — see [Websites](websites.md).

Read the [program terms](../README.md) first — reporting channels, reward
policy, KYC requirement, disclosure rules, and safe harbour apply to this
program.

> **Reporting channel.** Report findings against the hosted applications to
> [security@burnt.com](mailto:security@burnt.com). For `xion.js`, use GitHub
> private vulnerability reporting on that repository. Eligibility follows the
> asset, not the channel — the choice of channel does not affect it.

## Assets in Scope

**An asset is in scope only if it is listed below.** This list is exhaustive.
Nothing is in scope by resemblance, by sharing a domain, or by being reachable
from an asset that is listed.

### Hosted Applications

The **mainnet hostname identifies the scoped production asset**, but it is not an
authorized penetration-testing target. Perform active testing only against the
paired testnet hostname, a local instance, or infrastructure you control.

| Production asset — do not test   | Authorized testing hostname    | Application                                                        |
| -------------------------------- | ------------------------------ | ------------------------------------------------------------------ |
| `app.burnt.com`                  | `app.testnet.burnt.com`        | XION web app — staking, governance, and smart account transactions |
| `auth.burnt.com`                 | `auth.testnet.burnt.com`       | Authentication, account settings, and authenticator management     |
| `aa-api.mainnet.burnt.com`       | `aa-api.testnet.burnt.com`     | Account abstraction API                                            |

The testnet hostname authorizes testing; it is not a separate reward-eligible
asset. Submit the report against the paired production hostname and demonstrate
why the issue would affect that production application. A weakness caused only
by testnet-specific configuration, with no corresponding production impact, is
not reward eligible.

No other alias, preview URL, staging deployment, or test environment is
authorized by this table.

### Client SDK

| Repository | Covers |
| ---------- | ------ |
| [`burnt-labs/xion.js`](https://github.com/burnt-labs/xion.js) | Published `@burnt-labs/*` packages — Abstraxion meta account client library |

**SDK scope.** Only the published package code is in scope. Example apps,
demos, and test fixtures in the repository are not. A finding must be
exploitable in a dApp that consumes the package as released on npm, not only in
a local checkout with modified configuration.

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

### SDK Severity

The table above is written for hosted applications, where the attacker reaches a
victim through a page. `xion.js` is a library: the consumer is a developer, and
the victim is that developer's user. Severity for SDK findings is assessed on
what a correctly-integrated dApp is exposed to.

A finding is severity-rated only if it is exploitable against an integration
that **follows the documented usage**. A weakness that requires the consuming
dApp to misuse the API, disable a documented safeguard, or supply attacker-
controlled values where the documentation calls for trusted ones is capped at
**Low** — that is a documentation gap, and we would still like to know, but the
defect is in the integration rather than the package.

Signing, session key handling, and transaction construction are the paths that
carry High and Critical. A flaw that causes a dApp to sign a payload other than
the one presented to the user, that leaks or extends the life of a session key,
or that lets a third party influence transaction contents, qualifies regardless
of how many dApps are known to be affected.

## Proof of Concept

A proof of concept demonstrating the vulnerability is required. **Do not test or
demonstrate it against the production hostname** — testing production
disqualifies the report. For hosted applications, use the paired testnet hostname
listed above, a local instance, or infrastructure you control.

Screenshots or a video walkthrough showing end-to-end exploitation are expected
for High and Critical reports on hosted applications. Reports consisting only of
automated scanner output, without demonstrated exploitability, are not rewarded.

For `xion.js`, the proof of concept is a **minimal dApp that consumes the
published package** and exhibits the defect, plus the package version and the
integration code. State which `@burnt-labs/*` package and version you tested. A
diff against the repository, or a description of the flawed code path without a
running integration that exercises it, is not sufficient on its own.

## Out of Scope

**Assets and environments**

**Any production hostname not listed in the scope table is out of scope, and any
testing hostname not explicitly paired in that table is unauthorized.** The
following are called out only because they are the cases we are asked about most
often — the list is illustrative, not exhaustive.

- Testnet, staging, preview, and development environments other than the three
  authorized testing hostnames listed above
- Administrative, internal, and operational interfaces
- Faucets
- Alternate hostnames that reach an in-scope application. Report the finding
  against the canonical production hostname in the scope table instead
- Informational and marketing web properties — see [Websites](websites.md)
- Public blockchain RPC, REST, gRPC, and Tendermint RPC endpoints
- Backend APIs and services other than the account abstraction API listed above
- Example apps, demos, and test fixtures in `xion.js`, and any
  `@burnt-labs/*` package version that is not the current release
- Third-party services and infrastructure, including third-party dApps built
  with `xion.js`

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
