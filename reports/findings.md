# Intelligence Report — Cloudflare Infrastructure Mapping

**Classification:** Public  
**Date:** 2025-05-23  
**Analyst:** [Seu nome]  
**Method:** Passive OSINT — public data sources only  

---

## Executive Summary

This report presents the findings of a passive OSINT investigation
targeting the public digital infrastructure of Cloudflare, Inc.
The analysis mapped domains, subdomains, IP addresses, autonomous
system ownership, and email infrastructure using open source
intelligence techniques and visual link analysis.

No active scanning or unauthorized access was performed.
All data was obtained from publicly available sources.

---

## Confirmed Entities

### Domains and Subdomains

| Entity | Type | Status |
|---|---|---|
| cloudflare.com | Root domain | Active |
| api.cloudflare.com | API endpoint | Active |
| dash.cloudflare.com | Customer dashboard | Active |
| blog.cloudflare.com | Corporate blog | Active |
| cdnjs.cloudflare.com | Public CDN | Active |
| workers.cloudflare.com | Serverless platform | Active |
| radar.cloudflare.com | Threat intelligence platform | Active |
| cf-emailsecurity.net | Email security infrastructure | Active |

### IP Addresses

| IP | Associated Domain | Block |
|---|---|---|
| 104.16.132.229 | cloudflare.com | 104.16.0.0/12 |
| 104.16.133.229 | cloudflare.com | 104.16.0.0/12 |
| 104.19.192.176 | api.cloudflare.com | 104.16.0.0/12 |
| 104.17.110.184 | dash.cloudflare.com | 104.16.0.0/12 |
| 104.17.24.14 | cdnjs.cloudflare.com | 104.16.0.0/12 |
| 104.16.196.131 | workers.cloudflare.com | 104.16.0.0/12 |
| 104.18.30.78 | radar.cloudflare.com | 104.16.0.0/12 |
| 104.18.28.7 | blog.cloudflare.com | 104.16.0.0/12 |

### Network Infrastructure

| Entity | Value |
|---|---|
| ASN | AS13335 |
| IP Block | 104.16.0.0/12 |
| Total IPs in block | 1,048,576 |
| Organization | Cloudflare, Inc. (CLOUD14) |
| HQ | 101 Townsend Street, San Francisco, CA 94107 |

### Email Infrastructure (MX Records)

| Priority | Server |
|---|---|
| 5 | mxb-canary.global.inbound.cf-emailsecurity.net |
| 10 | mxa.global.inbound.cf-emailsecurity.net |
| 10 | mxb.global.inbound.cf-emailsecurity.net |

---

## Intelligence Findings

### Finding 1 — Full Vertical Integration

Cloudflare operates its entire public infrastructure on its own
network, using its own ASN, its own registrar, its own WHOIS server
and its own email security product for corporate communications.

This level of vertical integration means Cloudflare controls every
layer of its own digital infrastructure — from domain registration
to DNS resolution to traffic routing.

**Implication:** Any disruption to Cloudflare's own ASN would
simultaneously affect all its customer-facing services.

---

### Finding 2 — IP Block Segmentation by Service Function

Analysis of resolved IPs reveals deliberate segmentation
of services across sub-blocks within 104.16.0.0/12:

| Sub-block | Services |
|---|---|
| 104.16.x.x | cloudflare.com, workers.cloudflare.com |
| 104.17.x.x | dash.cloudflare.com, cdnjs.cloudflare.com |
| 104.18.x.x | radar.cloudflare.com, blog.cloudflare.com |
| 104.19.x.x | api.cloudflare.com |

This pattern suggests intentional network segmentation by
service tier — separating customer-facing APIs from
public content and internal tooling.

---

### Finding 3 — Anycast Routing Confirmed

All DNS lookups were performed simultaneously across multiple
global resolvers (OpenDNS, Google, Quad9, CenturyLink, Fortinet,
Yandex, Liquid Telecom). Every resolver returned identical IP
addresses for each domain.

This confirms Cloudflare uses global anycast routing — the same
IP addresses are announced from hundreds of data centers
simultaneously, with traffic routed to the nearest point of presence.

---

### Finding 4 — API Endpoint Load Distribution

api.cloudflare.com resolves to 6 simultaneous IP addresses,
compared to 2 for all other subdomains analyzed.

This indicates significantly higher traffic volume and redundancy
requirements for the API layer — consistent with serving millions
of customers performing continuous API calls.

---

### Finding 5 — Canary Deployment Server Exposed via Public DNS

The MX record for cloudflare.com reveals a server named
`mxb-canary.global.inbound.cf-emailsecurity.net`.

The "canary" designation indicates a canary deployment —
a server receiving live traffic while running a newer software
version ahead of general rollout. This internal architectural
detail is publicly visible via DNS.

---

### Finding 6 — Anomalous Behavior from Yandex DNS Resolver

The Yandex LLC DNS resolver returned IPs from the 8.47.69.0
and 8.6.112.0 ranges for some Cloudflare subdomains, diverging
from all other global resolvers.

This behavior is consistent with sovereign DNS policies or
localized DNS caching practices applied by Russian infrastructure,
and does not reflect Cloudflare's actual infrastructure.

---

## Limitations

- Analysis limited to publicly resolvable infrastructure
- ASN entity not natively supported in Maltego Community Edition
- Full subdomain enumeration was not possible due to crt.sh API instability
- IP-to-identity attribution beyond ARIN records was not performed

---

## Conclusion

The passive OSINT investigation successfully mapped Cloudflare's
public digital infrastructure, revealing clear patterns of vertical
integration, network segmentation, anycast architecture and
operational security practices.

All findings are based exclusively on public data and demonstrate
the analytical value of structured OSINT methodology even without
active reconnaissance tools.
