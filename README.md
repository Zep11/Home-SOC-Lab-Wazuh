# 🛡️ HOME SOC LAB    -       <img src="https://img.shields.io/badge/License-MIT-green">

## Windows Threat Detection & Incident Response Lab

A hands-on SOC laboratory built to simulate, detect, investigate, and correlate a multi-stage Windows attack using **Kali Linux, Windows 11, Sysmon, and Wazuh**.

The project demonstrates the complete SOC workflow:

**Attack Simulation → Detection → Investigation → MITRE ATT&CK Mapping → Incident Response → Attack Correlation**

---

## 🏗️ Lab Architecture

```text
Kali Linux
192.168.56.105
     │
     │ RDP / Attack Simulation
     ▼
Windows 11
192.168.56.106
     │
     │ Sysmon + Windows Logs
     ▼
Wazuh Agent
     │
     ▼
Wazuh Manager
     │
     ▼
Wazuh Dashboard
```

The lab was deployed using an isolated **Host-Only network** to prevent attacks from reaching the physical host.

---

## 🧰 Technologies Used

- **Kali Linux** — Attack simulation
- **Windows 11** — Target endpoint
- **Sysmon** — Endpoint telemetry
- **Wazuh** — SIEM & detection
- **MITRE ATT&CK** — Threat mapping
- **VirtualBox** — Lab virtualization

---

## 🎯 Objectives

- Build an isolated home SOC environment
- Collect Windows Security and Sysmon telemetry
- Detect suspicious endpoint activity using Wazuh
- Investigate Windows authentication and process activity
- Map activity to MITRE ATT&CK
- Correlate multiple events into a single incident
- Practice SOC investigation and incident response

---

# 🔥 Simulated Attack Chain

```text
RDP Brute Force
       ↓
Successful RDP Authentication
       ↓
PowerShell Execution
       ↓
Account Discovery
       ↓
Scheduled Task Persistence
       ↓
Defense Evasion
       ↓
Event Log Clearing
```

---

## 🔎 Detection Coverage

### 1. RDP Brute Force

Multiple failed authentication attempts were simulated against the Windows endpoint.

**Windows Event:** `4625`

**MITRE ATT&CK:** `T1110 — Brute Force`

---

### 2. Successful RDP

A successful Remote Desktop session was established after the authentication attempts.

**Windows Event:** `4624`

**Logon Type:** `10 — Remote Interactive`

**Source:** `192.168.56.105`

**Target:** `192.168.56.106`

**User:** `WIN11\vboxuser`

**MITRE ATT&CK:** `T1021.001 — Remote Services: RDP`

---

### 3. Post-RDP PowerShell Execution

Sysmon recorded PowerShell process creation after the successful RDP session.

**Sysmon Event:** `1 — Process Create`

**Wazuh Rule:** `92027`

**MITRE ATT&CK:** `T1059.001 — PowerShell`

---

### 4. Account Discovery

The authenticated session executed:

```text
net.exe localgroup administrators
```

This command was used to enumerate members of the local Administrators group.

**Wazuh Rule:** `92033`

**MITRE ATT&CK:** `T1087 — Account Discovery`

---

### 5. Scheduled Task Persistence

A scheduled task was created as part of the controlled persistence simulation:

```text
\SOC-Lab-persistence-Test
```

**Windows Event:** `4698`

**Wazuh Rule:** `60228`

**MITRE ATT&CK:** `T1053 — Scheduled Task/Job`

---

### 6. Defense Evasion

The Windows Application event log was cleared.

**Windows Event:** `104`

**Wazuh Rule:** `63104`

**MITRE ATT&CK:** `T1070 — Indicator Removal`

This demonstrated detection of an attempt to remove forensic evidence.

---

# 🧠 MITRE ATT&CK Mapping

| Activity | Technique | ID |
|---|---|---|
| RDP Brute Force | Brute Force | T1110 |
| Successful RDP | Remote Services: RDP | T1021.001 |
| PowerShell | PowerShell | T1059.001 |
| Account Discovery | Account Discovery | T1087 |
| Scheduled Task | Scheduled Task/Job | T1053 |
| Event Log Clearing | Indicator Removal | T1070 |

---

# 🔗 Attack Correlation

The investigation correlated the activity using the source IP, target endpoint, authenticated user, process relationships, and timestamps.

```text
192.168.56.105
       │
       ▼
RDP Authentication Failures
       │
       ▼
Successful RDP
       │
       ▼
WIN11\vboxuser
       │
       ▼
PowerShell
       │
       ▼
Account Discovery
       │
       ▼
Scheduled Task
       │
       ▼
Event Log Clearing
```

This transformed individual Wazuh alerts into a single multi-stage security incident.

---

# 🛡️ Incident Response

Recommended response actions for an equivalent unauthorized incident:

- Validate the source IP and RDP session
- Identify and secure the affected account
- Terminate the suspicious session
- Review the PowerShell process tree
- Investigate discovery activity
- Inspect newly created scheduled tasks
- Preserve Windows and Sysmon evidence
- Investigate event-log clearing
- Isolate the endpoint if compromise is confirmed

---

# 📊 SOC Skills Demonstrated

- Wazuh SIEM
- Sysmon
- Windows Security Event Analysis
- RDP Investigation
- PowerShell Investigation
- Process Tree Analysis
- Threat Hunting
- Detection Engineering
- MITRE ATT&CK Mapping
- Incident Correlation
- Incident Response

---

# 🧪 Lab Safety

The project was conducted inside an isolated virtualized environment using a Host-Only network.

The simulated attacks were directed only against the laboratory Windows VM and not the physical host.

---

# 🏁 Final Result

The HOME SOC LAB successfully demonstrated an end-to-end Windows attack investigation:

> **RDP Brute Force → Successful RDP → PowerShell → Discovery → Persistence → Defense Evasion**

The project demonstrates the ability to collect endpoint telemetry, investigate security events, detect attacker behavior, correlate multiple stages of an attack, map activity to MITRE ATT&CK, and perform incident-response analysis.

---


## 📌 Project Status

**Status:** ✅ Completed

**Environment:** Windows 11 + Kali Linux + Wazuh + Sysmon

**Focus:** Windows Threat Detection & Incident Response

**Project Type:** Home SOC / Blue Team Laboratory

## 👤 Author

**@Shubrajit Dey - Zep11**   - SOC Aspirant 

