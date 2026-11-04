# THM SOC & SIEM — Practical Investigation Portfolio

**Author:** Alen Sales K S  
**Focus:** SOC Analyst L1 | Blue Team | SIEM | Security Monitoring | Incident Response

This repository documents practical SOC and SIEM investigations completed in TryHackMe training environments. Each entry records the scenario, alert evidence, investigation steps, analysis, classification, escalation reasoning, indicators, remediation recommendations, and lessons learned.

## Completed investigation

### TryHackMe SOC Simulator — Email Phishing & Firewall Investigation
**Date:** 4 November 2026

Investigated inbound email alerts, suspicious URLs, a malicious URL reputation result, and a firewall-blocked outbound request. Correlated the Amazon-themed phishing email with the matching firewall event and assessed a separate Microsoft-themed credential-phishing email.

- [Open the full investigation report](SOC-Simulator/2026-11-04-Email-Phishing-Firewall-Investigation.md)
- [Open the SOC Simulator investigation index](SOC-Simulator/README.md)

## Repository layout

```text
Thm-Soc-SIEM/
├── README.md
└── SOC-Simulator/
    ├── README.md
    └── 2026-11-04-Email-Phishing-Firewall-Investigation.md
```

## Investigation areas demonstrated

- SOC alert triage and severity assessment
- Email and phishing analysis
- URL reputation analysis
- Firewall event interpretation
- Source/destination IP and port analysis
- Cross-source event correlation
- True-positive and false-positive decisions
- Incident escalation and remediation planning
- MITRE ATT&CK mapping
- Evidence-based case documentation

## Documentation approach

Reports are based on the evidence available in each lab. Confirmed observations are separated from assumptions; missing telemetry and attribution gaps are documented rather than guessed. Screenshots are included when the actual image files are available and committed to the repository.

## Responsible use

All activities documented here are performed in authorized training environments or on systems for which permission has been granted. Lab findings should not be interpreted as proof of compromise outside the training environment.
