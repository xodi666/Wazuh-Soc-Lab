# 🛡️ Open-Source SIEM Lab: Wazuh Detection Engineering & Threat Hunting

## 📌 Project Overview
This project documents the end-to-end setup, detection engineering, and incident analysis of an open-source SIEM environment using **Wazuh**, based on the 7-part MYDFIR hands-on lab path.

The objective was to deploy a centralized SIEM, enroll endpoints, develop custom XML detection rules, simulate adversary behavior, and conduct post-incident threat hunting.

---

## 🏗️ Lab Architecture

```text
[ Attacker Machine (Kali / Fleet) ]
               │
               ▼ (Simulated SSH Brute-Force & Attacks)
┌───────────────────────────────────────────────┐
│               Monitored Agents                │
│  • Linux Endpoint (SSH Auth Logs)             │
│  • Windows Endpoint (Security Event ID 4722)  │
└───────────────────────┬───────────────────────┘
                        │
                        ▼ (Telemetry & Syslog)
┌───────────────────────────────────────────────┐
│               Wazuh SIEM Stack                │
│  • Wazuh Manager (Analysis & Correlation Engine)│
│  • Custom Detection Rules (local_rules.xml)   │
│  • Wazuh Dashboard (Alerting & Visualization) │
└───────────────────────────────────────────────┘