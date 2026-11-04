# TryHackMe SOC Simulator — Email Phishing & Firewall Investigation

**Author:** Alen Sales K S  
**Platform:** TryHackMe SOC Simulator  
**Investigation date:** 4 November 2026  
**Scenario focus:** Email security, phishing triage, URL reputation, firewall investigation, event correlation, incident response

> **Evidence standard:** This report records only the alert details and tool results supplied for this lab. A suspicion is not presented as proof of compromise. Where telemetry was not supplied, the limitation is explicitly documented.

## Executive summary

This exercise involved triaging inbound email alerts and a related outbound firewall event. The investigation covered an onboarding-themed email, an Amazon-themed delivery notification, a Microsoft-themed account-security notification, and a firewall event involving a URL rated **MALICIOUS** by TryDetectThis.

The most important correlation was the exact Bitly URL shared by the Amazon-themed email and the firewall event. The firewall recorded the request as **BLOCKED**. This demonstrates an attempted connection that was blocked; it does not prove successful access, credential submission, malware execution, or endpoint compromise.

## Scenario dashboard results

The supplied TryHackMe dashboard screenshot displayed:

| Metric | Result |
|---|---:|
| True-positive identification rate | 100% |
| False-positive identification rate | 100% |
| Closed alerts | 5 |
| Mean time to resolve | 3 minutes |
| Mean dwell time | 8 minutes |

The dashboard reports five closed alerts. Four alert/event records are described in detail in this report; the fifth alert's details were not included in the supplied case information. These percentages and timings are simulator-reported metrics, not independently calculated measurements.

## Investigation index

| Case | Alert / event | Assessment |
|---|---|---|
| 01 | 8814 — Inbound Email Containing Suspicious External Link | False Positive, based on available evidence |
| 02 | Amazon delivery notification email | True Positive — suspected phishing |
| 03 | 8816 — Access to Blacklisted External URL Blocked by Firewall | True Positive — malicious URL access attempt |
| 04 | Microsoft account security notification email | True Positive — suspected credential phishing |

---

## Case 01 — Alert 8814: Inbound Email Containing Suspicious External Link

**Severity:** Medium  
**Category:** Phishing  
**Email timestamp:** 4 November 2026, 17:22:56.649  
**URL analysis activity shown:** 4 November 2026, 22:55

### Email evidence

| Field | Value |
|---|---|
| Sender | onboarding@hrconnex.thm |
| Recipient | j.garcia@thetrydaily.thm |
| Subject | Action Required: Finalize Your Onboarding Profile |
| Direction | Inbound |
| Attachment | None |
| Embedded URL | https://hrconnex.thm/onboarding/15400654060/j.garcia |
| TryDetectThis result | CLEAN |

### Investigation

1. Reviewed the sender, recipient, subject, and email body.
2. Examined the onboarding-related context of the message.
3. Extracted the embedded URL and submitted it to TryDetectThis.
4. Recorded the returned result as **CLEAN**.

### Classification and rationale

**False Positive — based on the evidence available in this exercise.**

The message content was consistent with an employee onboarding notification, no attachment was present, and the URL analysis tool returned CLEAN. No confirmed malicious indicator was provided in the available evidence.

### Limitations and follow-up

A CLEAN reputation result does not prove that a URL is safe. The sender's legitimacy was not independently verified, and no firewall or proxy log results were supplied to confirm whether an endpoint accessed the URL.

- Verify the sender and onboarding request through a trusted channel.
- Review firewall and proxy logs for access attempts.
- Reassess the classification if new threat intelligence or suspicious activity is discovered.

---

## Case 02 — Amazon-Themed Delivery Phishing Email

**Email timestamp:** 4 November 2026, 17:26:09.649  
**Category:** Phishing

### Email evidence

| Field | Value |
|---|---|
| Sender | urgents@amazon.biz |
| Recipient | h.harris@thetrydaily.thm |
| Subject | Your Amazon Package Couldn’t Be Delivered – Action Required |
| Direction | Inbound |
| Attachment | None |
| Embedded URL | http://bit.ly/3sHkX3da12340 |

### Investigation

1. Reviewed the sender domain and noted that it was not an obvious Amazon-owned domain.
2. Identified the delivery-failure pretext and 48-hour deadline as urgency-based social engineering.
3. Extracted the shortened Bitly URL; its final destination was not visible in the email.
4. Identified the potential for credential or personal-information harvesting.

### Classification and rationale

**True Positive — suspected phishing.**

The sender identity, delivery-themed pretext, artificial urgency, and concealed URL destination are consistent with a phishing attempt. The firewall event in Case 03 contains the same URL, and the URL analysis tool returned MALICIOUS, providing additional evidence that the link was dangerous.

### Escalation rationale

The message impersonates a trusted e-commerce brand and encourages the recipient to follow a potentially malicious link. Investigation is required to determine whether the recipient or another user accessed it and whether any information was submitted.

### Recommended response

- Quarantine or remove the email.
- Investigate and block the malicious URL and related indicators through appropriate security controls.
- Search the email environment for similar messages.
- Review proxy, DNS, and firewall logs for access attempts.
- If interaction is identified, investigate endpoint activity and determine whether credentials or personal information were submitted.

### Attack indicators

- Sender: `urgents@amazon.biz`
- URL: `http://bit.ly/3sHkX3da12340`
- Theme: Fake package delivery failure
- Social engineering: Urgency and threatened package return
- Relevant MITRE ATT&CK technique: **T1566.002 — Spearphishing Link**

---

## Case 03 — Alert 8816: Access to Blacklisted External URL Blocked by Firewall

**Severity:** High  
**Category:** Firewall  
**Event timestamp:** 4 November 2026, 17:27:23.649

### Firewall evidence

| Field | Value |
|---|---|
| Action | BLOCKED |
| Source IP | 10.20.2.17 |
| Source port | 34257 |
| Destination IP | 67.199.248.11 |
| Destination port | 80 |
| URL | http://bit.ly/3sHkX3da12340 |
| Application | web-browsing |
| Protocol | TCP |
| Firewall rule | Blocked Website |
| TryDetectThis result | MALICIOUS |

### Investigation

1. Examined the firewall event and identified source and destination addresses and ports.
2. Confirmed that the firewall action was **BLOCKED**.
3. Submitted the URL to TryDetectThis and recorded the result as **MALICIOUS**.
4. Compared the URL with the Amazon-themed email in Case 02 and found an exact URL match.

### Classification and rationale

**True Positive — malicious URL access attempt.**

The URL analysis tool classified the URL as MALICIOUS, and the firewall log recorded an outbound request to that URL. The firewall successfully blocked the connection.

### Escalation rationale

An internal endpoint attempted to access a URL identified as malicious. This activity may be related to interaction with the phishing email documented in Case 02. The source device and user must be identified, and endpoint telemetry should be reviewed to determine whether any additional activity occurred.

### Recommended response

- Identify the host and user associated with source IP `10.20.2.17`.
- Correlate endpoint identity with the recipient of the Amazon-themed email.
- Review browser, DNS, proxy, and endpoint telemetry.
- Keep the malicious URL blocked using appropriate controls.
- Search for further attempts to reach the same destination or related infrastructure.
- Investigate for compromise only if additional evidence supports it.

### Attack indicators

- Source IP: `10.20.2.17`
- Destination IP: `67.199.248.11`
- Destination port: `80`
- URL: `http://bit.ly/3sHkX3da12340`
- Protocol: TCP
- Firewall action: BLOCKED
- URL analysis result: MALICIOUS
- Related email: Amazon-themed delivery phishing email (Case 02)
- Relevant MITRE ATT&CK technique: **T1566.002 — Spearphishing Link** (possible delivery vector; user interaction is not independently confirmed)

### Investigation limitation

The blocked event proves an attempted connection, not successful access, malware execution, credential theft, or endpoint compromise. The source IP has not been independently mapped to the email recipient.

---

## Case 04 — Microsoft-Themed Account Security Phishing Email

**Email timestamp:** 4 November 2026, 17:28:27.649  
**Category:** Credential phishing

### Email evidence

| Field | Value |
|---|---|
| Sender | no-reply@m1crosoftsupport.co |
| Recipient | c.allen@thetrydaily.thm |
| Subject | Unusual Sign-In Activity on Your Microsoft Account |
| Direction | Inbound |
| Attachment | None |
| Embedded URL | https://m1crosoftsupport.co/login |
| Claimed sign-in location | Lagos, Nigeria |
| Claimed sign-in IP | 102.89.222.143 |

### Investigation

1. Examined the sender domain and identified a lookalike spelling: the number `1` replaces the letter `i` in Microsoft.
2. Reviewed the account-security pretext and urgent instruction to secure the account.
3. Extracted the embedded login URL and noted that it uses the same suspicious lookalike domain.
4. Assessed the potential for credential harvesting.

### Classification and rationale

**True Positive — suspected credential phishing.**

The lookalike sender domain, deceptive Microsoft branding, unexpected sign-in warning, and login link hosted on the suspicious domain are strong indicators of a credential-phishing attempt.

### Escalation rationale

If the recipient follows the link and submits credentials, an attacker may gain access to the user's Microsoft account and potentially organizational resources. Further investigation is required to establish whether the link was accessed or credentials were entered.

### Recommended response

- Quarantine the email and search for similar messages.
- Investigate and block the suspicious domain and URL using approved security controls.
- Review DNS, proxy, and firewall logs for requests to the domain.
- Review Microsoft Entra ID sign-in logs for suspicious authentication activity.
- If credential exposure is confirmed, reset credentials, revoke active sessions, and review MFA methods and account changes.
- Investigate the endpoint if the link was opened.

### Attack indicators

- Sender: `no-reply@m1crosoftsupport.co`
- Lookalike domain: `m1crosoftsupport.co`
- URL: `https://m1crosoftsupport.co/login`
- Claimed IP in email: `102.89.222.143`
- Claimed location in email: Lagos, Nigeria
- Technique: Brand impersonation and credential phishing
- Relevant MITRE ATT&CK technique: **T1566.002 — Spearphishing Link**

### Investigation limitation

The IP address and location are claims contained in the email; they have not been independently verified as a genuine sign-in event or attacker infrastructure.

---

## Cross-alert correlation — Case 02 and Case 03

The strongest correlation in this exercise is the exact URL shared by the Amazon-themed email and the firewall event.

| Evidence | Email event | Firewall event |
|---|---|---|
| URL | `http://bit.ly/3sHkX3da12340` | `http://bit.ly/3sHkX3da12340` |
| Time | 17:26:09 | 17:27:23 |
| Recipient / source | h.harris@thetrydaily.thm | 10.20.2.17 |
| Finding | Suspicious delivery email | Outbound request blocked |
| URL reputation | Later assessed as MALICIOUS | MALICIOUS |

**Assessment:** The exact URL match and close timestamps make the events strongly related. However, the available evidence does not prove that `10.20.2.17` belongs to `h.harris`. DHCP, asset inventory, identity, and endpoint records should be checked before attributing the network event to that recipient.

This demonstrates the value of correlating email and network telemetry rather than investigating alerts independently.

---

## Skills practised

- SOC L1 alert triage and prioritization
- Email sender and content analysis
- Phishing and brand-impersonation detection
- URL extraction and reputation analysis
- Firewall log interpretation
- Source and destination IP analysis
- Event correlation across email and network telemetry
- True-positive and false-positive classification
- Incident escalation and remediation planning
- MITRE ATT&CK mapping
- Evidence-based case documentation

## Key learning outcomes

1. An alert must be investigated before it is classified as malicious or benign.
2. A CLEAN URL reputation result is not a guarantee of safety.
3. Lookalike domains and shortened URLs are useful phishing indicators.
4. Firewall logs can establish whether a connection was allowed or blocked.
5. A blocked connection does not prove that an endpoint is compromised.
6. Correlation across email, firewall, DNS, and endpoint data improves investigation quality.
7. Analysts should distinguish confirmed evidence from assumptions and clearly document investigation gaps.

## Conclusion

This TryHackMe SOC Simulator exercise provided practical experience in investigating phishing-related email alerts and suspicious outbound network activity. It reinforced the importance of structured alert triage, threat-intelligence checks, firewall log analysis, event correlation, accurate classification, and evidence-based escalation.

The most significant finding was the correlation between the Amazon-themed phishing email and a firewall-blocked request to the same URL, which was rated MALICIOUS by the URL analysis tool. The exercise also highlighted the need to verify endpoint ownership and user interaction before concluding that a user or device was compromised.

All investigations were conducted in the authorized TryHackMe training environment for educational purposes.

## References

- [TryHackMe SOC Simulator](https://tryhackme.com/soc-sim/dashboard)
- [MITRE ATT&CK — T1566.002: Spearphishing Link](https://attack.mitre.org/techniques/T1566/002/)
