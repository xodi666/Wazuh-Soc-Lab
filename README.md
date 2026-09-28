##### **#** 🛡️ **Open-Source SIEM Lab: Wazuh Detection Engineering \& Threat Hunting**

##### 

##### **## 📌 Project Overview**

##### This project documents the end-to-end setup, detection engineering, automated threat response, and incident analysis of an open-source SIEM environment using \*\*Wazuh\*\*, based on the 7-part MYDFIR hands-on lab path.

##### 

##### The objective was to deploy a centralized SIEM, enroll Linux and Windows endpoints, engineer custom XML detection rules, simulate adversary behavior, configure automated active response, and conduct post-incident threat hunting.

##### 

##### \---

##### 

##### **## 🏗️ Lab Architecture**

##### 

##### ```text

##### \[ Attacker Machine (192.168.20.133) ]

##### &#x20;              │

##### &#x20;              ▼ (Simulated SSH Brute-Force \& Attacks)

##### ┌──────────────────────────────────────────────────┐

##### │                Monitored Agents                  │

##### │  • Linux Endpoint (MYDFIR-LINUX / 192.168.20.132)│

##### │  • Windows Endpoint (MYDFIR-WINDOWS / .20.133)   │

##### └────────────────────────┬─────────────────────────┘

##### &#x20;                        │

##### &#x20;                        ▼ (Telemetry \& Syslog)

##### ┌──────────────────────────────────────────────────┐

##### │                Wazuh SIEM Stack                  │

##### │  • Wazuh Manager (Analysis \& Correlation Engine) │

##### │  • Custom Detection Rules (local\_rules.xml)      │

##### │  • Automated Active Response Engine              │

##### │  • Wazuh Dashboard (Alerting \& Visualization)    │

##### └──────────────────────────────────────────────────┘

##### 

##### \---

##### 

##### **## 🎯 MITRE ATT\&CK Framework Mapping**

##### 

##### | Tactic | Technique ID | Technique Name | Custom Detection Rule ID | Rule Level |

##### | :--- | :--- | :--- | :--- | :--- |

##### | \*\*Credential Access\*\* | \[T1110](https://attack.mitre.org/techniques/T1110/) | Brute Force | `100101` | 10 |

##### | \*\*Persistence\*\* | \[T1078](https://attack.mitre.org/techniques/T1078/) | Valid Accounts | `100200` | 12 |

##### | \*\*Persistence\*\* | \[T1098](https://attack.mitre.org/techniques/T1098/) | Account Manipulation | `100200` | 12 |

##### 

##### \---

##### 

##### \## **⚙️ Custom Detection Rules \& Active Response**

##### 

##### \### 1. **SSH Brute-Force Threshold Rule (`Rule ID: 100101`)**

##### Detects 3 or more failed SSH login attempts originating from the same source IP within a 120-second window:

##### 

##### ```xml

##### <rule id="100101" level="10" frequency="3" timeframe="120">

##### &#x20; <if\_matched\_group>authentication\_failed</if\_matched\_group>

##### &#x20; <same\_source\_ip />

##### &#x20; <description>Multiple SSH login failures observed from the same source IP</description>

##### &#x20; <mitre>

##### &#x20;   <id>T1110</id>

##### &#x20; </mitre>

##### &#x20; <group>authentication\_failed,ssh\_bruteforce,credential\_access</group>

##### </rule>

##### 

##### 

##### 

##### **Windows Account Persistence Detection (Rule ID: 100200)**

##### 

##### Monitored Event ID 4722 (User Account Enabled) specifically targeting built-in Guest account activation.

##### 

##### 

##### 

##### **Automated Active Response Configuration**

##### 

##### Configured Wazuh Active Response in ossec.conf to automatically execute a firewall drop against offending source IPs triggering Rule 100101 for 600 seconds:

##### 

##### <active-response>

##### &#x20; <command>firewall-drop</command>

##### &#x20; <location>local</location>

##### &#x20; <rules\_id>100101</rules\_id>

##### &#x20; <timeout>600</timeout>

##### </active-response>

##### Wazuh Dashboard Alert Telemetry

##### 

##### Below is the dashboard telemetry verifying successful alert generation for custom Rules 100101 and 100200:

##### 

##### 

##### Rule Validation via wazuh-logtest

##### 

##### Rule 100101 was validated using wazuh-logtest by passing raw SSH failure logs to confirm threshold execution on the 3rd log occurrence:

##### 

##### 

##### 📁 Repository Structure

##### 

##### &#x20;   config/local\_rules.xml: Custom rules deployed to /var/ossec/etc/rules/local\_rules.xml.

##### 

##### &#x20;   config/active\_response\_snippet.xml: Active Response XML snippet for automated firewall drop.

##### 

##### &#x20;   logs/: Raw log samples generated during adversary simulation (ssh\_bruteforce\_raw.log, windows\_event\_4722.json).

##### 

##### &#x20;   reports/incident-report-01.md: SOC Analyst incident post-mortem report.

##### 

##### &#x20;   screenshots/: High-resolution proof-of-concept screenshots.

