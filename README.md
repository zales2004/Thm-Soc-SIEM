# THM SOC & SIEM — Hands-on Investigation Portfolio

**Author:** Alen Sales K S  
**Focus:** SOC Analyst L1 | SIEM | Blue Team | Incident Investigation

## About this repository

This repository is the central portfolio for my TryHackMe SOC and SIEM practical exercises. It records hands-on investigations in a consistent, professional format, with emphasis on evidence, alert triage, event correlation, detection reasoning, MITRE ATT&CK mapping, and remediation.

Each completed simulator or SOC/ SIEM exercise should be added here as a dated investigation, with relevant screenshots and supporting evidence where available.

## Investigations

### SOC Simulator — Email Phishing & Firewall Investigation
**Date:** 4 November 2026

Investigated inbound email alerts, suspicious URLs, phishing indicators, a firewall-blocked outbound request, and cross-alert correlation.

- [Read the full investigation report](SOC-Simulator/2026-11-04-Email-Phishing-Firewall-Investigation.md)
- [View the dashboard evidence](evidence/README.md)

## Repository structure

```text
Thm-Soc-SIEM/
├── README.md
├── SOC-Simulator/
│   ├── README.md
│   ├── 2026-11-04-Email-Phishing-Firewall-Investigation.md
│   └── CASE-REPORT-TEMPLATE.md
└── evidence/
    └── README.md
```

## Standard for future uploads

For each new SOC/SIEM simulation, I will add:

1. A dated folder or report named after the scenario.
2. Scenario overview, objective, severity, and data sources.
3. Individual alert/case details and investigation steps.
4. Evidence-based classification and escalation rationale.
5. Relevant IOCs, log fields, queries, and MITRE ATT&CK mapping where supported.
6. Recommended containment, remediation, and follow-up actions.
7. Relevant screenshots, clearly captioned and referenced from the report.
8. Lessons learned and investigation limitations.

Screenshots should show useful lab evidence such as alert details, query results, tool output, investigation timelines, or final simulator metrics. Secrets, tokens, personal data, and unrelated browser information should be excluded or redacted.

## Skills practised

- SOC alert triage and prioritization
- SIEM and security event investigation
- Email and phishing analysis
- URL reputation and threat-intelligence checks
- Firewall and network log analysis
- Event correlation
- Incident escalation and documentation
- MITRE ATT&CK mapping

## Responsible use

All exercises documented here are performed in authorized training environments or on systems for which permission has been granted. Findings are limited to the evidence available in each lab and should not be interpreted as proof of compromise outside that environment.

---

This is a living practical portfolio. New SOC, SIEM, detection, and incident-response exercises will be added as they are completed.
