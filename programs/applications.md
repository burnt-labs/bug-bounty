# Bug Bounty — Applications and SDKs

Covers the production applications operated by Burnt Labs, and the client SDK
third-party developers build against, where a vulnerability could result in user
harm, credential compromise, or unauthorized transaction execution.

Informational and marketing web properties are covered separately, at a lower
tier — see [Websites](websites.md).

Read the [program terms](../README.md) first — reporting channels, reward
policy, KYC requirement, disclosure rules, and safe harbour apply to this
program.

> **Reporting channel.** Most repositories behind these applications are
> private, so GitHub private vulnerability reporting is not available for them.
> Report findings in this program to
> [security@burnt.com](mailto:security@burnt.com). Eligibility follows the asset,
> not the channel — reporting by email does not affect it. `xion.js` is public
> and does accept private vulnerability reporting.

## Assets in Scope

An asset is in scope only if it appears below. For hosted applications the
**mainnet hostname is the asset** — the source repository is named to identify
the code, not to extend scope to everything that repository builds.

### Hosted Applications

| Hostname                          | Application                                                        |
| --------------------------------- | ------------------------------------------------------------------ |
| `app.burnt.com`                   | XION web app — staking, governance, and smart account transactions |
| `auth.burnt.com`                  | Authentication and smart account creation                          |
| `auth-beta.burnt.com`             | Authentication (beta)                                              |
| `auth-beta.mainnet.burnt.com`     | Authentication (beta)                                              |
| `settings.burnt.com`              | Account settings and authenticator management                      |
| `aa-api.mainnet.burnt.com`        | Account abstraction API                                            |
| `aa-api.xion-mainnet-1.burnt.com` | Account abstraction API                                            |

The code behind these hostnames lives in `burnt-labs/xion-frontends`
(`app.burnt.com`), `burnt-labs/xion-dashboard-app` (`auth`, `auth-beta`, and
`settings`), and `burnt-labs/account-abstraction-api` (`aa-api`). The first two
are private.

**`xion-frontends` note.** That repository is a monorepo, and only
`apps/xion-app`, serving `app.burnt.com`, is in scope. The other applications it
builds — `apps/xion-admin` (`dev.burnt.com`) and `apps/faucet`
(`faucet.xion.burnt.com`) — are out of scope as a developer portal and a faucet
respectively.

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

A proof of concept demonstrating the vulnerability is required. **Do not
demonstrate it against production** — testing production disqualifies the
report. Run the application locally or against your own deployment.

Screenshots or a video walkthrough showing end-to-end exploitation are expected
for High and Critical reports on hosted applications. Reports consisting only of
automated scanner output, without demonstrated exploitability, are not rewarded.

For `xion.js`, the proof of concept is a **minimal dApp that consumes the
published package** and exhibits the defect, plus the package version and the
integration code. State which `@burnt-labs/*` package and version you tested. A
diff against the repository, or a description of the flawed code path without a
running integration that exercises it, is not sufficient on its own.

## Out of Scope

**Assets**

- Any hostname not listed above, including testnet and staging hosts
  (`*.testnet.burnt.com` and equivalents), the developer portal at
  `dev.burnt.com`, admin interfaces, and internal tooling
- The faucet at `faucet.xion.burnt.com`
- The block explorer at `explorer.burnt.com`, `explorer.mainnet.burnt.com`, and
  `explorer.xion.burnt.com`. It is derived from an upstream open source project
  and is not currently covered by any program
- Informational and marketing web properties — see [Websites](websites.md)
- Decommissioned properties that are no longer in use, including the staking
  interface at `staking.burnt.com` and its variants
- Public blockchain RPC, REST, gRPC, and Tendermint RPC endpoints
- Backend APIs and services other than the account abstraction API named above
- Example apps, demos, and test fixtures in `xion.js`, and any
  `@burnt-labs/*` package version that is not the current release
- Third-party services and infrastructure, and third-party dApps built with
  `xion.js`

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
