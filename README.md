# OSINT Infrastructure Mapping — Cloudflare

> Open Source Intelligence analysis using Maltego Community Edition to map and correlate Cloudflare's public digital infrastructure.

---

## Objective

Map publicly available infrastructure data related to Cloudflare, Inc.,
correlating domains, subdomains, IPs, ASN, and email infrastructure
using OSINT techniques and visual link analysis via Maltego CE.

---

## Methodology

This project follows a structured OSINT investigation workflow:

1. **Subdomain Enumeration** — Certificate Transparency logs via crt.sh
2. **DNS Analysis** — Active A and MX record lookup via dnschecker.org
3. **Whois / RDAP** — Domain registration and IP ownership via who.is
4. **ASN Mapping** — Autonomous System identification and IP block analysis
5. **Link Analysis** — Entity correlation and graph construction via Maltego CE

All data collected from public sources only. No active scanning performed.

Every data point is recorded in the [collection log](data/raw/collection-log.md) with its
source, query, and timestamp — the provenance record that makes the collection reproducible.

---

## Analytic standards applied

OSINT is governed by a different standards set than media-based forensics: there is no
acquisition, no seized artifact, and no chain of custody in the traditional sense. What
governs an intelligence product instead is **sourcing discipline** and **analytic rigour**.

| Framework | Application in this investigation |
|---|---|
| **Admiralty Code** (NATO STANAG 2511) | Each source graded for reliability (A–F) and each item for credibility (1–6). See the Source Reliability Assessment in the report. |
| **ICD 203** — Analytic Standards | Explicit separation of observed data from analytic judgment; every finding carries a stated confidence level (high / moderate / low). |
| **ISO/IEC 27043** | Investigation process principles — scope and objectives defined before collection began, preventing an unfocused sweep. |
| **LGPD / GDPR** | Purpose limitation and necessity applied to operational contact data surfaced in public registries; no enrichment or attribution to individuals. |
| **Lei 12.737/2012 (Art. 154-A)** | The passive-only method was chosen so that no query touches target systems — the legal boundary between open-source consultation and unauthorized access. |

---

## Tools

| Tool | Purpose |
|---|---|
| Maltego Community Edition | Link analysis and graph construction |
| crt.sh | Certificate transparency / subdomain enumeration |
| dnschecker.org | DNS record lookup (A, MX) |
| who.is | Whois and IP ownership lookup |

---

## Key Findings

> Findings are confidence-graded in the [full report](reports/findings.md).
> Two are high-confidence, two moderate, and two are recorded at low confidence and not relied upon.

- Cloudflare operates its entire public infrastructure on its own ASN (AS13335)
- IP space is segmented by service function within the 104.16.0.0/12 block (1M+ IPs)
- All services use anycast routing — consistent global DNS resolution confirmed
- Corporate email runs on cf-emailsecurity.net — Cloudflare's own email security product
- Cloudflare self-registers its own domains using its own registrar and WHOIS server
- Canary deployment server identified via public MX record (mxb-canary)

---

## Graph

![Maltego Graph](graphs/maltego-graph-cloudflare-main.png)

---


## Disclaimer

This project was conducted exclusively using public data sources.  
No systems were accessed without authorization.  
All findings are based on openly available information.

---

## Author

Paulo Vaz

https://www.linkedin.com/in/paulohvz/
