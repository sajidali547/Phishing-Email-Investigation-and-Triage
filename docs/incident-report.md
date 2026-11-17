# Incident Report: Apple-Themed Phishing Simulation

## Summary and verdict

**Verdict: True Positive — phishing training sample.**

The message impersonates Apple Support and pressures the recipient to verify an account through a lookalike website. The verdict is supported by the sender/link context, coercive wording, and a phishing classification in the captured URL report.

The `.eml` was reconstructed from the supplied exercise and screenshot. Its headers were authored for training; they are not proof of delivery through Google's infrastructure. The sample contains a URL and no attachment.

## Header findings

| Check | Evidence |
| --- | --- |
| From and Return-Path | Both use `apple-id-verification-support[.]com`; their domains match each other. The suspicious feature is Apple branding with a lookalike domain. |
| SPF | The sample declares `spf=fail`. MXToolbox separately reports **No SPF record found**. |
| DKIM | The sample declares `dkim=none`; MXToolbox reports no `DKIM-Signature` header. |
| DMARC | The sample declares `dmarc=fail`; MXToolbox separately reports **No DMARC Record found**. |

An absent SPF record is distinct from an SPF `fail` result. DMARC alignment concerns authenticated domains, rather than a displayed brand name. These distinctions prevent overclaiming what the screenshot establishes. [SPF specification](https://www.rfc-editor.org/rfc/rfc7208.html#section-2.6), [DMARC specification](https://www.rfc-editor.org/rfc/rfc9989.html).

## Indicators and URL review

Indicators below are defanged for documentation.

| Indicator | Role |
| --- | --- |
| `security@apple-id-verification-support[.]com` | Claimed sender. |
| `bounce@apple-id-verification-support[.]com` | Return-Path. |
| `support@apple-id-verification-support[.]com` | Reply-To. |
| `hxxp://apple-id-verify-login[.]com/secure` | Verification link in the sample and VirusTotal screenshot. |

**VirusTotal snapshot:** 14 November 2026, 06:18:28 UTC; **1/93 detections**, including **ESET: Phishing**. This is a captured result, not a new scan or a claim that multiple vendors flagged the link. It supports the review but does not establish the behavior of a live landing page.

The sample IP `198.51.100.45` belongs to the documentation-only TEST-NET-2 range. It is not an established attacker address or a production blocking indicator. [RFC 5737](https://www.rfc-editor.org/rfc/rfc5737.html#section-3).

## Social engineering

- **Impersonation:** the sender presents itself as Apple Support.
- **Pressure:** verification is demanded within 12 hours.
- **Threat:** the account will supposedly be permanently disabled.
- **Generic greeting:** the recipient is addressed as "Dear Customer".
- **Suspicious destination:** the verification link points outside `apple.com`.

This exercise illustrates link-based phishing. ATT&CK reference: [T1566.002 — Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/).

## Recommended response

For a comparable confirmed incident, a SOC would:

1. Quarantine the message, preserve the original, and search for related messages using the sender and URL.
2. Validate the actual indicators before applying mail or web-filter blocks.
3. Check whether the recipient clicked the link or entered credentials. Review browsing and endpoint activity after a click; reset affected credentials and revoke sessions if credentials were disclosed.

These are recommendations. No production blocking, account changes, or user compromise is claimed in this lab.

## Evidence

[Header review](../screenshots/01-header-analysis.png) · [URL report](../screenshots/02-virustotal-url.png) · [Email red flags](../screenshots/03-email-red-flags.png) · [Training completion](../screenshots/04-tryhackme-proof.png).
