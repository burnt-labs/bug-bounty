# Bug Bounty — Websites

Covers the informational and marketing web properties operated by Burnt Labs.
These properties do not hold user sessions, manage keys, or construct
transactions.

This is a **lower tier than [Applications and SDKs](applications.md)**, and the
severity ladder below reflects that. If a finding concerns `app.burnt.com`,
`auth.burnt.com`, `settings.burnt.com`, the account abstraction API, or
`xion.js`, it belongs to that program instead.

Read the [program terms](../README.md) first — reporting channels, reward
policy, KYC requirement, disclosure rules, and safe harbour apply to this
program.

> **Reporting channel.** Report findings in this program to
> [security@burnt.com](mailto:security@burnt.com).

## Assets in Scope

Unlike the other programs, this one is **not defined by an enumerated list**.
Informational properties come and go — campaign pages, event microsites,
documentation sections — and an allowlist would go stale faster than we could
maintain it.

A property is in scope if **all** of the following hold:

1. It is operated by Burnt Labs and reachable on the public internet
2. It is a production property, not a preview, staging, or testnet deployment
3. It is **informational or marketing in nature** — it presents content. It does
   not authenticate users, hold sessions, manage keys or authenticators, or
   construct, sign, or broadcast transactions

The third condition is the boundary that matters. The moment a property does any
of those things it is an application, and it is in scope only if it is named in
[Applications and SDKs](applications.md) — not here, and not by default.

The clearest examples are the marketing site at `burnt.com` and `www.burnt.com`,
and the developer documentation at `docs.burnt.com`.

If you are unsure which program a property falls under, report it and say so.
We would rather triage the question than lose the finding.

**Third-party hosting.** Several of these properties run on third-party
platforms — `docs.burnt.com` is hosted on GitBook, for example. Only Burnt Labs'
own content and configuration are in scope. Vulnerabilities in the underlying
platform should be reported to that platform.

## Severity

These properties hold nothing an attacker wants directly. The risk that matters
is a visitor being served attacker-controlled content and carrying the
consequences to a property that *does* hold something — connecting a wallet,
following a link to an authentication flow, or downloading a tampered artifact.
Severity is assessed on that path, not on the website in isolation.

| Severity     | Description                                                                                                                                                                                                                       |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CRITICAL** | Not reachable in this program. A finding that warrants Critical necessarily involves an asset in another program — report it there                                                                                                 |
| **HIGH**     | Persistent attacker-controlled script or content served to ordinary visitors, where the demonstrated consequence is theft of funds or credentials — a wallet drainer, a convincing redirect into a credential-harvesting authentication flow, or a tampered download. Subdomain takeover of an in-scope hostname |
| **MEDIUM**   | Reflected or self-limiting content injection requiring an unusual link or a security-relevant victim action. Exposure of non-public but non-sensitive information. Access control bypass on a content management path              |
| **LOW**      | Valid security issue with no demonstrated path to user harm, representing a meaningful hardening opportunity                                                                                                                       |

Only **High** is reward eligible in this program.

A finding is not High merely because injection is possible. The report must
carry it through to the consequence — show the drainer, the harvest, or the
tampered artifact.

## Proof of Concept

A proof of concept demonstrating the vulnerability is required. **Do not
demonstrate it against production** — testing production disqualifies the
report. Run the property locally or against your own deployment.

Screenshots or a video walkthrough are expected for High reports, showing the
full path from injection to consequence. Reports consisting only of automated
scanner output, without demonstrated exploitability, are not rewarded.

For a subdomain takeover claim, include the dangling record and evidence you
could serve content — do not serve anything beyond a benign proof file, and do
not leave it in place.

## Out of Scope

**Assets**

- Any property failing one of the three conditions above. In particular:
  preview, staging, and testnet deployments; the developer portal at
  `dev.burnt.com`; admin interfaces; faucets; and internal tooling, whether or
  not it is reachable publicly
- Anything that authenticates users, holds sessions, manages keys or
  authenticators, or touches transactions. Those are applications, and are in
  scope only where [Applications and SDKs](applications.md) names them
- The block explorer. It is derived from an upstream open source project and is
  not currently covered by any program
- The GitBook platform, and any third-party analytics, forms, chat widgets, or
  embeds loaded by these properties
- Decommissioned properties that are no longer in use
- Social media accounts, community channels, and content published on
  third-party platforms

**Vulnerability classes**

- Missing security headers, cookie flags, and TLS configuration findings where
  no exploitable impact on a visitor is demonstrated
- Clickjacking. These properties carry no state-changing actions
- Open redirects that do not leak tokens or credentials
- Self-XSS requiring the attacker to execute code in their own browser session
- Content spoofing and text injection without script execution
- Email configuration findings — SPF, DKIM, and DMARC records
- Rate limiting, enumeration, and denial of service
- Outdated software versions without a demonstrated working exploit
- Issues requiring physical access to the visitor's device
- Social engineering
- Best practices and informational findings
