# 🛡️ End-to-End Windows Attack Simulation, Detection & SOC Investigation Lab
> **Atomic Red Team ➔ Sysmon ➔ Azure Arc / AMA ➔ Microsoft Sentinel**

---

## 📌 Project Overview
This repository documents the architecture, simulation, telemetry pipeline, and Security Operations Center (SOC) investigation workflow for an enterprise-grade Windows security monitoring lab.

By executing threat simulations using **Atomic Red Team** mapped to the **MITRE ATT&CK Framework**, security telemetry is generated on a target endpoint (**Windows 11 VM** with **Sysmon64** & **Windows Security Logs**) and ingested into **Microsoft Sentinel** via **Azure Monitor Agent (AMA)** for KQL threat hunting, incident triage, and root cause analysis.

---

## 🎨 System Architecture & Telemetry Pipeline

```text
1. ATTACK SIMULATION LAYER
   ├── Kali Linux VM (RDP / T1021.001)
   └── Windows 11 VM (Invoke-AtomicTest Execution)

2. ENDPOINT TELEMETRY LAYER
   ├── Sysmon64 (Event IDs 1, 13, 104)
   └── Windows Security Log (Event IDs 4624, 4688)

3. CLOUD & SIEM INGESTION LAYER
   ├── Azure Arc & Azure Monitor Agent (AMA Pipeline)
   └── Microsoft Sentinel Workspace (SecurityEvent, Event, SigninLogs)
```
## 🔍 The 3-Tier Investigation Methodology

Every executed scenario follows a strict verification methodology to evaluate visibility across local endpoint logs and SIEM data lakes:

* **Stage A — Host Ground Truth (Sysmon / PowerShell):** Validate execution directly at the kernel/process level on the endpoint using local event logs (`EVTX`) before network transport.
* **Stage B — SIEM Data Ingestion (Microsoft Sentinel):** Query central log tables (`SecurityEvent`, `Event`) via KQL to verify Azure Monitor Agent (AMA) ingestion and schema mapping.
* **Stage C — Incident Triage & Correlation:** Evaluate behavioral correlation, rule triggers, and incident generation within Microsoft Sentinel.

---

## 📂 Scenario Case Studies Index

Click on any scenario below to view its full technical breakdown, commands, KQL queries, and forensic findings.

| Tactic | Technique ID | Technique Name | Full Investigation Writeup |
| :--- | :--- | :--- | :--- |
| Execution | T1059.003 | Windows Command Shell | [📁 View Case Study](./scenarios/T1059.003.md) |
| Persistence | T1053.005 | Scheduled Task | [📁 View Case Study](./scenarios/T1053.005.md) |
| Privilege Escalation | T1548.002 | UAC Bypass (fodhelper) | [📁 View Case Study](./scenarios/T1548.002.md) |
| Defense Evasion | T1070.001 | Clear Event Logs | [📁 View Case Study](./scenarios/T1070.001.md) |
| Discovery | T1082 | System Info Discovery | [📁 View Case Study](./scenarios/T1082.md) |
| Lateral Movement | T1021.001 | Remote Desktop (RDP) | [📁 View Case Study](./scenarios/T1021.001.md) |

---

## 📋 Standard Operating Procedure: Incident Response Workflow

When an alert or suspicious process is detected in Sentinel, apply this 5-stage framework:

1. **Triage:** Inspect process command lines, parent-child relationships, and token elevation levels in the `SecurityEvent` or `Event` tables.
2. **Scope:** Perform time-series pivoting across Sentinel logs (`SecurityEvent`, `SigninLogs`) to identify affected accounts and hosts.
3. **MITRE Mapping:** Classify adversary techniques against standard ATT&CK IDs (`T1059.003`, `T1053.005`) to determine intent.
4. **Response:** Isolate host, terminate malicious process trees, and revoke compromised credentials.
5. **Closure:** Classify the incident (*True Positive*, *False Positive*, or *Benign Positive*) and document root-cause findings.

---

## 🛠️ Key Technologies & Lab Environment

* **SIEM / Analytics:** Microsoft Sentinel Workspace
* **Endpoint Telemetry:** Sysmon64 (Modular Config), Windows Security Event Logs
* **Log Transport:** Azure Arc, Azure Monitor Agent (AMA), Data Collection Rules (DCR)
* **Emulation & Testing:** Atomic Red Team (`Invoke-AtomicTest`), PowerShell, Kali Linux
