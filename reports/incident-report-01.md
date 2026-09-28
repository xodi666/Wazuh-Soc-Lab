# SOC Incident Investigation Report: IR-2026-001

## 1. Executive Summary
During simulated adversary testing on the monitored endpoint (`agent.id: 002`), the Wazuh SIEM detected sequential brute-force attempts targeting the SSH service followed by local privilege escalation / persistence activity. Custom correlation rules flagged these events as high-severity alerts.

## 2. Event Timeline
| Timestamp | Event / Alert Description | Rule ID | Level | Mitre Technique |
| :--- | :--- | :--- | :--- | :--- |
| 13:21:36 | PAM / SSH login failed | 5716 | 5 | Credential Access |
| 13:21:38 | SSHD authentication failure detected | 5716 | 5 | Credential Access |
| 13:21:42 | SSHD authentication failure detected | 5716 | 5 | Credential Access |
| 13:21:50 | **Multiple SSH login failures observed from same IP** | **100101** | **10** | **T1110 (Brute Force)** |
| 13:23:00 | Windows Built-in Guest account enabled | 100200 | 12 | T1078 / T1098 |

## 3. Scope & Analysis
- **Target Host:** Agent 002 (Linux / Windows Endpoints)
- **Attack Vector:** Password brute-force attack via SSH.
- **Key Finding:** Rule `100101` successfully aggregated individual failed authentication events (`5716`) within a 120-second timeframe to trigger a high-severity alert for SOC review.

## 4. Remediation & Recommendations
1. Configure Wazuh **Active Response** (`firewall-drop`) to automatically block IPs triggering Rule `100101`.
2. Disable password-based SSH authentication in favor of SSH key pairs.
3. Enforce strict Audit Policies for local account modifications on Windows endpoints.