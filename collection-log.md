# Collection Log — Provenance Record

This log records **how and when each data point was obtained**, so that any third party can reproduce the collection and arrive at the same raw data.

In a media-based forensic investigation this role is served by a chain-of-custody document. Open-source intelligence has no seized artifact to take custody of — the equivalent discipline is **provenance**: recording the source queried, the exact query issued, the timestamp of collection, and whether the source is authoritative or a third-party relay. Without this, a finding cannot be independently verified, and OSINT data is perishable (DNS records, WHOIS entries, and certificate logs all change over time).

---

## Collection sessions

| # | Date | Source | Query issued | Data obtained | Output file |
|---|---|---|---|---|---|
| 1 | 2025-05-22 | crt.sh | `%.cloudflare.com` | Subdomain candidates from Certificate Transparency logs | (verified via session 2) |
| 2 | 2025-05-22 | dnschecker.org | A record lookup, per subdomain, multi-resolver | IPv4 addresses for 7 subdomains across 7 global resolvers | `dns-findings.md` |
| 3 | 2025-05-23 | dnschecker.org | MX record lookup, `cloudflare.com` | 3 mail servers with priority values | `dns-mx-findings.md` |
| 4 | 2025-05-23 | who.is | `cloudflare.com` | Registrar, WHOIS server, creation/expiry dates, nameservers | `whois-findings.md` |
| 5 | 2025-05-23 | who.is | `104.16.132.229` | CIDR, NetName, Org, RegDate, operational contacts | `whois-findings.md` |
| 6 | 2025-05-23 | who.is | `as13335.com` | Registrar, creation/expiry dates, nameservers | `whois-findings.md` |
| 7 | 2025-05-23 | Maltego CE | Manual entity import from sessions 1–6 | Link-analysis graph | `graphs/` |

---

## Source authority classification

A distinction that materially affects how much weight each finding can carry:

| Source | Authority | Note |
|---|---|---|
| crt.sh | **Authoritative (relay)** | Certificate Transparency logs are cryptographically verifiable and append-only by design; crt.sh relays them but the underlying data cannot be silently altered. |
| dnschecker.org | **Third-party relay** | Queries public resolvers on the analyst's behalf. Not authoritative itself, but querying 7 independent resolvers provides internal cross-validation. |
| who.is | **Third-party relay** | Presents WHOIS/RDAP data sourced from the registry. The *underlying* ARIN and registrar records are authoritative; who.is is a convenience layer over them. For a formal product, queries should be re-run directly against ARIN RDAP and the registrar's WHOIS server. |
| Maltego CE | **Analysis tool** | Produces no primary data in this investigation; used exclusively for visual correlation of data collected above. |

---

## Reproducibility notes

- **Data is time-bound.** All records reflect the state of Cloudflare's public infrastructure on 2025-05-22/23. DNS records and IP assignments change; a re-run at a later date is expected to produce different IPs while preserving the structural findings (ASN ownership, anycast behaviour, vertical integration).
- **crt.sh instability.** The crt.sh API was intermittently unavailable during session 1, preventing exhaustive subdomain enumeration. Subdomains were therefore confirmed individually via DNS resolution rather than accepted from the certificate log alone — a slower method, but one that verifies each subdomain is actually resolving rather than merely having had a certificate issued at some point.
- **No active scanning.** No port scans, service probes, or connection attempts were made against target infrastructure. All queries were directed at public lookup services, not at Cloudflare systems.
