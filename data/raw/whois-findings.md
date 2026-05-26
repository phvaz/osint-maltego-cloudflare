# Whois Findings — Cloudflare Infrastructure

**Tool:** who.is  
**Date:** 2025-05-23

## cloudflare.com

- Registrar: Cloudflare, Inc. (self-registered)
- WHOIS Server: whois.cloudflare.com (self-hosted)
- Created: 2009-02-17
- Expires: 2033-02-17
- Nameservers: ns3–ns7.cloudflare.com → 162.159.x.x block

## IP 104.16.132.229

- CIDR: 104.16.0.0/12 (1,048,576 IPs)
- NetName: CLOUDFLARENET
- Org: Cloudflare, Inc. (CLOUD14)
- RegDate: 2014-03-28
- Address: 101 Townsend Street, San Francisco, CA 94107, US
- Abuse: abuse@cloudflare.com
- NOC: noc@cloudflare.com
- RIR contact: rir@cloudflare.com

## AS13335 (as13335.com)

- Registrar: Cloudflare, Inc. (self-registered)
- Created: 2013-05-01
- Expires: 2032-05-01
- Nameservers: ns1–ns5.as13335.com → 162.159.x.x block

## Key findings

- Full vertical integration: Cloudflare registers and resolves its own domains
- 162.159.x.x block serves as nameserver infrastructure for all Cloudflare-owned domains
- CIDR 104.16.0.0/12 confirms ownership of 1M+ IPs under AS13335
- Physical HQ confirmed: 101 Townsend Street, San Francisco, CA
- Operational contacts exposed via ARIN: abuse, NOC, RIR
- as13335.com registered 4 years after cloudflare.com — brand protection of ASN identifier
