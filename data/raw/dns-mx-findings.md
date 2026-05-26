# MX Record Findings — cloudflare.com

**Tool:** dnschecker.org  
**Date:** 2025-05-23  
**Record type:** MX

## Results

| Priority | Server |
|---|---|
| 5 | mxb-canary.global.inbound.cf-emailsecurity.net |
| 10 | mxa.global.inbound.cf-emailsecurity.net |
| 10 | mxb.global.inbound.cf-emailsecurity.net |

## Key findings

- Email infrastructure runs on cf-emailsecurity.net — Cloudflare's own email security product
- Dogfooding pattern confirmed: corporate email protected by their own commercial product
- "canary" in mxb-canary hostname indicates a canary deployment server — internal versioning detail exposed via public DNS
- Priority 5/10 structure indicates primary server with redundant fallback
- Global resolution fully consistent — same servers returned worldwide
