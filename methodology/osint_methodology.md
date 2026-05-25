
# OSINT Methodology — Cloudflare Infrastructure Mapping

## Overview

This document describes the investigative methodology applied  
in this project, following structured OSINT principles.

---

## Phase 1 — Target Definition

**Objective:** Define the investigation scope before any data collection.

- Target: Cloudflare, Inc. (cloudflare.com)
- Scope: Public digital infrastructure only
- Constraints: No active scanning, no unauthorized access
- Data sources: Open source, publicly available only

---

## Phase 2 — Passive Reconnaissance

### 2.1 Subdomain Enumeration
- **Tool:** crt.sh (Certificate Transparency logs)
- **Method:** Query `%.cloudflare.com` to identify subdomains
  via publicly issued SSL certificates
- **Rationale:** Certificate transparency logs are public by design,
  revealing subdomains that may not appear in standard DNS queries

### 2.2 DNS Analysis
- **Tool:** dnschecker.org
- **Records collected:** A (IPv4 addresses), MX (mail servers)
- **Method:** Active lookup from multiple global resolvers simultaneously
- **Rationale:** Multi-resolver queries confirm anycast routing
  and reveal infrastructure consistency across regions

### 2.3 Whois / RDAP
- **Tool:** who.is
- **Targets:** cloudflare.com (domain), 104.16.132.229 (IP), AS13335 (ASN)
- **Rationale:** Whois records confirm ownership, registration dates,
  IP block allocation and operational contacts

---

## Phase 3 — Data Correlation

### 3.1 Entity Mapping
All collected data points were classified as entities:
- Domains and subdomains
- IPv4 addresses
- ASN (Autonomous System Number)
- Organization
- Mail servers

### 3.2 Relationship Identification
Relationships were established based on:
- DNS resolution (domain → IP)
- IP ownership (IP → ASN → Organization)
- Mail routing (domain → MX → mail infrastructure)
- Certificate issuance (domain → subdomain)

---

## Phase 4 — Link Analysis (Maltego)

- **Tool:** Maltego Community Edition
- **Method:** Manual entity import and relationship mapping
- **Graph structure:** Hierarchical — root domain at top,
  expanding through subdomains, IPs, ASN and email infrastructure
- **Rationale:** Visual graph reveals infrastructure patterns
  not immediately obvious in raw data

---

## Investigative Principles Applied

| Principle | Application |
|---|---|
| Passive reconnaissance | No active scanning performed |
| Source documentation | Every finding linked to its source |
| Evidence-based analysis | No entity added without confirmed data |
| Methodological transparency | Full process documented here |

---

## Limitations

- Maltego Community Edition restricts the number of transforms available
- crt.sh API instability required manual subdomain verification via DNS
- ASN entity not natively available in Maltego CE — represented via Phrase entity
- Analysis limited to publicly resolvable infrastructure
