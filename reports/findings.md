# Intelligence Report — Cloudflare Infrastructure Mapping

**Classification:** Public  
**Date:** 2025-05-23  
**Analyst:** Paulo Vaz  
**Method:** Passive OSINT — public data sources only  
**Analytic standards:** Admiralty Code (source reliability / information credibility); ICD 203 analytic tradecraft (fact–judgment separation, expressed confidence levels)  
**Reference frameworks:** ISO/IEC 27043 (investigation process); LGPD / GDPR (personal data in open sources)  

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

## Source Reliability Assessment

Intelligence products are only as strong as the sources beneath them. Each source used in this investigation is graded using the **Admiralty Code** (NATO STANAG 2511), the standard rating system for intelligence sourcing: a letter for **source reliability** (A = completely reliable → F = cannot be judged) and a numeral for **information credibility** (1 = confirmed by other sources → 6 = cannot be judged).

| Source | Rating | Justification |
|---|---|---|
| Certificate Transparency (crt.sh) | **A2** | CT logs are append-only and cryptographically verifiable by design — the record cannot be silently altered. Rated 2 rather than 1 because certificate issuance proves a name was certified, not that it currently resolves. |
| DNS resolution via 7 independent global resolvers | **B1** | dnschecker.org is a third-party relay rather than an authoritative source, but identical responses from seven independent resolvers provide strong internal confirmation. |
| WHOIS / RDAP via who.is | **B2** | The underlying ARIN and registrar records are authoritative (A-grade); who.is is a convenience layer over them. Rated B because the relay was not independently verified against ARIN RDAP directly. |
| Maltego CE graph | **n/a** | An analysis and visualization tool. It generates no primary data in this investigation and therefore carries no independent source rating. |

**Implication for this report:** no finding rests on a source rated below B, and the strongest findings (1, 3) rest on multi-source confirmation. The single most improvable point is the WHOIS chain — re-querying ARIN RDAP and the registrar's WHOIS server directly would raise that data from B2 to A1.

---

## A note on findings vs. judgments

A recurring failure in intelligence reporting is presenting an analyst's *inference* with the same confidence as *observed data*. This report separates the two explicitly, following ICD 203 analytic standards. Each finding below carries a stated confidence level:

- **High confidence** — the conclusion follows directly from observed data; alternative explanations are not credible.
- **Moderate confidence** — the conclusion is well supported but involves inference; alternative explanations exist but are less likely.
- **Low confidence** — the conclusion is plausible and consistent with the data, but the evidence base is thin or competing explanations are equally viable. Stated for completeness, not relied upon.

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

**Confidence: High.** This is a direct reading of registry data, not an inference.
WHOIS records independently confirm self-registration (registrar and WHOIS server
both Cloudflare), ARIN confirms ASN and IP block ownership, and MX records confirm
the email product. No alternative explanation is credible.

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

This pattern is *consistent with* intentional network segmentation by
service tier, but does not establish it.

**Confidence: Low.** This is the weakest finding in the report and is stated for
completeness rather than relied upon. The sample is eight subdomains within a
1,048,576-address block, and the apparent grouping is equally consistent with
routine address allocation, internal hashing, or the order in which services were
provisioned. Two subdomains sharing a /16 is not evidence of design intent.
Confirming this finding would require a substantially larger sample and, ideally,
correlation with Cloudflare's published network documentation.

---

### Finding 3 — Anycast Routing Confirmed

All DNS lookups were performed simultaneously across multiple
global resolvers (OpenDNS, Google, Quad9, CenturyLink, Fortinet,
Yandex, Liquid Telecom). Every resolver returned identical IP
addresses for each domain.

This confirms Cloudflare uses global anycast routing — the same
IP addresses are announced from hundreds of data centers
simultaneously, with traffic routed to the nearest point of presence.

**Confidence: High.** Identical resolution across seven independent resolvers on
different continents is direct observational evidence, not inference. Anycast is
also consistent with Cloudflare's publicly documented architecture, providing
external corroboration.

---

### Finding 4 — API Endpoint Load Distribution

api.cloudflare.com resolves to 6 simultaneous IP addresses,
compared to 2 for all other subdomains analyzed.

This indicates significantly higher traffic volume and redundancy
requirements for the API layer — consistent with serving millions
of customers performing continuous API calls.

**Confidence: Moderate.** The observation (6 IPs vs. 2) is directly observed data.
The interpretation — that this reflects higher traffic volume — is a reasonable
inference grounded in standard load-distribution practice, but it is an inference:
a larger address pool may also reflect deployment topology, historical allocation,
or client-side load-balancing strategy rather than raw volume alone.

---

### Finding 5 — Canary Deployment Server Exposed via Public DNS

The MX record for cloudflare.com reveals a server named
`mxb-canary.global.inbound.cf-emailsecurity.net`.

The "canary" designation is consistent with a canary deployment —
a server receiving live traffic while running a newer software
version ahead of general rollout. If so, this internal architectural
detail is publicly visible via DNS.

**Confidence: Moderate.** "Canary" is a well-established industry naming convention
for staged rollouts, and its appearance at priority 5 (ahead of the two priority-10
servers) is consistent with a subset of live mail being routed to it first — the
expected behaviour for a canary. However, the conclusion rests on interpreting a
hostname; the naming could be vestigial, or used for an unrelated internal purpose.
No traffic-level evidence was collected to confirm the deployment behaviour itself.

---

### Finding 6 — Anomalous Behavior from Yandex DNS Resolver

The Yandex LLC DNS resolver returned IPs from the 8.47.69.0
and 8.6.112.0 ranges for some Cloudflare subdomains, diverging
from all other global resolvers.

This behavior does not reflect Cloudflare's actual infrastructure — that much is
established by the six concurring resolvers. The *cause* of the divergence is not.

**Confidence: Low (as to cause).** The divergence itself is directly observed and
high-confidence. The explanation is not: sovereign DNS policy, ISP-level caching,
transparent DNS interception, upstream resolver misconfiguration, and localized
CDN redirection would all produce this signature. The returned ranges (8.47.69.0,
8.6.112.0) are not Cloudflare-allocated, which narrows the possibilities but does
not distinguish between them. Attributing the behaviour specifically to state
policy would exceed what the collected data supports; it is recorded here as an
anomaly requiring further investigation, not as an established finding.

---

## Limitations

Each limitation is stated together with its concrete impact on the findings, so that its practical significance — not merely its existence — is clear.

- **Analysis limited to publicly resolvable infrastructure.** *Impact:* the map is necessarily partial. Internal networks, non-published services, and infrastructure behind unadvertised addresses are invisible to this method. This bounds every finding to "Cloudflare's *public* posture" and means no negative claim can be made — the absence of an entity in this report is not evidence of its non-existence.

- **Full subdomain enumeration not achieved (crt.sh API instability).** *Impact:* the eight subdomains analyzed are a confirmed subset, not the complete set. This directly weakens Finding 2 (the segmentation hypothesis rests on this small sample) and means the IP-block pattern may not hold across the full estate. Findings 1, 3, and 5 are unaffected, as they do not depend on sample completeness.

- **WHOIS data collected via a third-party relay rather than authoritative registries.** *Impact:* the underlying records are authoritative, but the relay was not independently verified against ARIN RDAP. This caps the source rating at B2 (see Source Reliability Assessment). No finding is currently contradicted, but for a formal product this chain should be re-run directly against ARIN and the registrar.

- **ASN entity not natively supported in Maltego Community Edition.** *Impact:* presentational only. AS13335 was represented via a Phrase entity in the graph. The underlying ASN data is unaffected; only the visual fidelity of the graph is reduced.

- **No traffic-level or temporal analysis performed.** *Impact:* directly bounds Findings 4 and 5. Both interpret infrastructure configuration (IP count, hostname) as evidence of operational behaviour (traffic volume, deployment strategy). Without traffic observation those remain inferences, which is why both are rated Moderate rather than High.

- **Data is a point-in-time snapshot (2025-05-22/23).** *Impact:* specific IP addresses and record values are perishable and will not reproduce on a later re-run. The structural findings (ASN ownership, anycast behaviour, vertical integration) are expected to be stable; the address-level detail is not.

- **IP-to-identity attribution beyond registry records was not performed.** *Impact:* the investigation establishes organizational ownership of infrastructure, not the identity or activity of any individual. No conclusion in this report makes, or can support, a claim about a person.

---

## Legal and Ethical Context

Open-source intelligence is bounded less by technical capability than by legal and ethical constraint. The methodology applied here was deliberately chosen to remain well inside those boundaries, and the reasoning is recorded so it can be assessed rather than assumed.

- **Passive collection only.** No port scans, service enumeration, vulnerability probes, or connection attempts were directed at target infrastructure. Every query was issued to a public lookup service (crt.sh, dnschecker.org, who.is), not to Cloudflare systems. This distinction is legally material: passive consultation of published records does not constitute unauthorized access, whereas active probing of systems without authorization may, under Brazil's Lei 12.737/2012 (Art. 154-A, *invasão de dispositivo informático*) and comparable statutes elsewhere. The method was selected to keep the investigation on the correct side of that line by design, not by accident.

- **Personal data considerations (LGPD / GDPR).** The collection surfaced operational contact addresses (`abuse@`, `noc@`, `rir@cloudflare.com`) and a registered corporate address. Although published in public registries, contact data can constitute personal data where it identifies or relates to an identifiable natural person. In this report such data is retained only where it evidences organizational ownership — its legitimate investigative purpose — and no attempt was made to enrich, cross-reference, or attribute it to individuals. Under the LGPD, public availability does not by itself authorize unrestricted processing; purpose limitation and necessity still apply.

- **Target selection.** Cloudflare was selected as a subject precisely because its infrastructure is intentionally public, extensively documented, and operated by an organization with mature security practices. No individual, small entity, or party with a reasonable expectation of obscurity was targeted. This matters: the same techniques applied to a private individual would raise materially different ethical questions even where the data is equally public.

- **Evidentiary posture.** Should OSINT of this kind be used to support a formal proceeding, its weight would depend on provenance rather than content: the collection log records the source, query, and timestamp for each data point, and the source reliability assessment states the confidence attaching to each. Because open-source data is perishable and mutable, contemporaneous provenance documentation — not the finding itself — is what makes it defensible later. This report is an academic exercise and does not constitute a legal instrument in any jurisdiction.

---

## Conclusion

The passive OSINT investigation mapped Cloudflare's public digital
infrastructure across domains, addressing, autonomous system ownership
and email routing, using open sources exclusively.

Two findings are established with **high confidence**: Cloudflare's full
vertical integration across registration, resolution, routing and email
(Finding 1), and its use of global anycast routing (Finding 3). Both rest
on directly observed, multi-source data rather than inference.

Two further findings are supported at **moderate confidence** — the API
layer's elevated address allocation (Finding 4) and the canary mail server
(Finding 5). Both interpret configuration as evidence of operational
behaviour, an inference that is reasonable but was not independently
confirmed.

Two findings are recorded at **low confidence** and are not relied upon:
the service-tier segmentation hypothesis (Finding 2), which rests on too
small a sample, and the cause of the Yandex resolver divergence (Finding 6),
where several competing explanations remain equally viable.

Stating these distinctions explicitly is itself part of the result. An
intelligence product that presents inference with the same weight as
observation is not more useful for being more confident — it is less
useful, because the reader cannot tell which conclusions would survive
scrutiny. The value of this investigation lies not only in what was mapped,
but in the discipline of separating what the data shows from what the
analyst concluded.
