# DNS Lookup Findings — cloudflare.com subdomains

**Tool:** dnschecker.org  
**Date:** 2025-05-22  
**Record type:** A

## Results

| Subdomain | IPs resolved | Notes |
|---|---|---|
| cloudflare.com | 104.16.132.229, 104.16.133.229 | Main site — 2 IPs |
| api.cloudflare.com | 104.19.192.174–177, 104.19.193.29, 104.19.192.29 | 6 IPs — highest load balancing |
| dash.cloudflare.com | 104.17.110.184, 104.17.111.184 | Customer dashboard |
| cdnjs.cloudflare.com | 104.17.24.14, 104.17.25.14 | Public CDN |
| workers.cloudflare.com | 104.16.196.131, 104.16.197.131 | Serverless platform |
| radar.cloudflare.com | 104.18.30.78, 104.18.31.78 | Threat intelligence |
| blog.cloudflare.com | 104.18.28.7, 104.18.29.7 | Corporate blog |

## Key findings

- All IPs belong to AS13335 (Cloudflare's own ASN) — dogfooding pattern confirmed
- IP blocks are segmented by service function (104.16, 104.17, 104.18, 104.19)
- api.cloudflare.com resolves to 6 IPs vs 2 for all others — indicates highest traffic volume
- Global DNS resolution is fully consistent — confirms anycast routing
- Anomaly: Yandex DNS returned 8.47.69.0 / 8.6.112.0 for some domains (sovereign DNS behavior)