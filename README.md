Markdown
# 🛡️ End-to-End Windows Attack Simulation, Detection & SOC Investigation Lab
> **Atomic Red Team ➔ Sysmon ➔ Defender XDR ➔ Azure Arc / AMA ➔ Microsoft Sentinel**

---

## 📌 Project Overview
This repository documents the architecture, simulation, telemetry pipeline, and Security Operations Center (SOC) investigation workflow for an enterprise-grade Windows security monitoring lab.

By executing threat simulations using **Atomic Red Team** mapped to the **MITRE ATT&CK Framework**, security telemetry is generated on a target endpoint (**Windows 11 VM** with **Sysmon64** & **Microsoft Defender**) and streamed into **Microsoft Sentinel** (SIEM) and **Microsoft Defender XDR** for KQL threat hunting, incident triage, and root cause analysis.

---

## 🎨 System Architecture & Telemetry Pipeline

ATTACK SIMULATION LAYER
├── Kali Linux VM (RDP / T1021.001)
└── Windows 11 VM (Invoke-AtomicTest Execution)

ENDPOINT TELEMETRY LAYER
├── Sysmon64 (Event IDs 1, 13, 104)
└── Microsoft Defender for Endpoint Sensor (DeviceProcessEvents)

CLOUD & SIEM INGESTION LAYER
├── Azure Arc & Azure Monitor Agent (AMA Pipeline)
├── Microsoft Defender XDR Portal (Alerts & Incidents)
└── Microsoft Sentinel Workspace (SecurityEvent, SigninLogs, Analytics)

## 🔍 The 3-Tier Investigation Methodology

Every executed scenario follows a strict three-tier verification methodology to evaluate visibility across local endpoint logs, SIEM data lakes, and EDR portals:

* **Stage A — Host Ground Truth (Sysmon / PowerShell):** Validate execution directly at the kernel/process level on the endpoint using local event logs before any network transport.
* **Stage B — SIEM Data Ingestion (Microsoft Sentinel):** Query central log tables via KQL to verify Azure Monitor Agent (AMA) ingestion, schema mapping, and parser performance.
* **Stage C — EDR & Alert Context (Defender XDR):** Evaluate telemetry enrichment, behavioral correlation, timeline aggregation, and incident generation in the Defender portal.

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

When an alert or suspicious process is detected, apply this 5-stage incident response framework:

1. **Triage:** Inspect process command lines, parent-child relationships, and token elevation levels for suspicious anomalies.
2. **Scope:** Perform time-series pivoting across Sentinel (`SecurityEvent`, `SigninLogs`) and the Defender Device Timeline.
3. **MITRE Mapping:** Classify adversary techniques against standard ATT&CK IDs (`T1059.003`, `T1053.005`) to guide investigative depth.
4. **Response:** In Defender XDR, execute containment actions: **Isolate Device**, **Stop & Quarantine Process**, or **Block Executable Hash**.
5. **Closure:** Classify the incident (*True Positive*, *False Positive*, or *Benign Positive*) and document root-cause findings.

---

## 🛠️ Key Technologies & Lab Environment
* **SIEM / SOAR:** Microsoft Sentinel Workspace
* **EDR / XDR:** Microsoft Defender for Endpoint / Microsoft Defender XDR
* **Endpoint Telemetry:** Sysmon64 (Modular Config), Windows Event Logs, Azure Monitor Agent (AMA)
* **Emulation & Testing:** Atomic Red Team (`Invoke-AtomicTest`), PowerShell, Kali Linux

