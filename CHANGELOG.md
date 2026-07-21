# Changelog

All notable changes to this investigation's documentation are recorded here.

Intelligence products are versioned rather than silently edited: once a report has been issued, subsequent changes are published as a new version with the modifications stated explicitly. This preserves the auditability of the documentation itself.

---

## [1.1] — 2026-07-16

Revision following technical review. **No collected data, entity values, or observations were altered.** All changes concern how findings are sourced, graded, and qualified.

### Added

- **`data/raw/collection-log.md`** — a provenance record documenting source, query issued, and timestamp for every collection session, plus a source-authority classification. This is the open-source equivalent of a chain-of-custody document: OSINT has no seized artifact to take custody of, so reproducibility rests on provenance instead.
- **Source Reliability Assessment (report).** Every source graded under the **Admiralty Code** (NATO STANAG 2511) — reliability A–F, credibility 1–6 — with justification for each rating and an explicit statement of the weakest link in the sourcing chain.
- **Confidence levels on every finding (report).** Following **ICD 203** analytic tradecraft standards, each of the six findings now carries a stated confidence level (high / moderate / low) distinguishing observed data from analytic judgment.
- **Legal and Ethical Context (report).** Passive-collection rationale and its legal basis (Lei 12.737/2012, Art. 154-A), LGPD/GDPR treatment of operational contact data surfaced in registries, target-selection reasoning, and the evidentiary posture of open-source material.
- **Analytic standards section (README)** mapping each framework to its application, and a **standards note** in the methodology document covering the ISO/IEC 27043 preparation stage.
- **Investigative principles** extended with source grading, fact–judgment separation, and provenance recording.

### Changed

- **Finding 2 (service-tier segmentation)** downgraded to **low confidence** and reworded from "suggests intentional segmentation" to "consistent with, but does not establish". The inference rests on eight subdomains within a 1,048,576-address block; routine allocation explains the pattern equally well.
- **Finding 5 (canary server)** reworded from "indicates a canary deployment" to "is consistent with", rated **moderate confidence** — the conclusion interprets a hostname and was not confirmed by traffic-level evidence.
- **Finding 6 (Yandex resolver divergence)** split into its observed and inferred components. The divergence is high-confidence; the *cause* is downgraded to **low confidence**, as sovereign DNS policy, ISP caching, transparent interception, and localized redirection all produce the same signature. The original wording attributed it to state policy more firmly than the data supports.
- **Limitations** expanded so each states its concrete impact on specific findings, rather than describing the limitation alone.
- **Conclusion** rewritten to report findings by confidence tier rather than as a uniform set.

---

## [1.0] — 2025-05-23

Initial release.

- Passive OSINT investigation of Cloudflare's public infrastructure via certificate transparency, DNS resolution, and WHOIS/RDAP.
- Eight domains and subdomains, eight IP addresses, ASN and IP block ownership, and email infrastructure mapped.
- Six intelligence findings documented.
- Link-analysis graph constructed in Maltego Community Edition.
