# 🛡️ End-to-End Windows Attack Simulation & Detection Lab
> **Atomic Red Team ➔ Sysmon ➔ Defender XDR ➔ Microsoft Sentinel**

---

## 📌 Project Overview
This repository details the architecture, execution, and detection pipeline of an enterprise-grade threat detection lab. By simulating real-world adversary behavior aligned with the **MITRE ATT&CK Framework** using **Atomic Red Team**, telemetry is captured at the endpoint layer (**Sysmon** & **Defender for Endpoint**) and ingested into **Microsoft Sentinel** for SIEM analysis and incident handling.

---

## 🎨 System Architecture & Telemetry Flow

```gantt
+-----------------------------------------------------------------------------------+
|                            🔴 ATTACK SIMULATION LAYER                            |
|                                                                                   |
|  [ 🖥️ Kali Linux ] ──────────── (RDP / T1021.001) ─────────────┐                  |
|                                                                │                  |
|  [ 🪟 Windows 11 VM ]                                           │                  |
|     └─► ⚡ Invoke-AtomicTest (Execution, Persistence, UAC, etc.) │                  |
+--------------------------------│-------------------------------+                  |
                                 │                                                  |
                                 ▼                                                  |
+-----------------------------------------------------------------------------------+
|                            🟡 ENDPOINT TELEMETRY LAYER                           |
|                                                                                   |
|  [ 🪟 Windows 11 VM ]                                                             |
|     ├─► 📜 Sysmon (Event ID 1, 13, 104) ──► (Local EVTX)                        |
|     └─► 🛡️ Defender for Endpoint Sensor  ──► (DeviceProcessEvents)                |
+--------------------------------│-------------------------------+                  |
                                 │                                                  |
                                 ▼                                                  |
+-----------------------------------------------------------------------------------+
|                            🟢 CLOUD & SIEM INGESTION LAYER                        |
|                                                                                   |
|  [ ☁️ Azure Arc / DCR Pipeline ]                                                    |
|     │                                                                             |
|     ├─► 🚨 Microsoft Defender XDR (security.microsoft.com)                        |
|     │      └─► Incident Triage & Alerts                                           |
|     │                                                                             |
|     └─► 📊 Microsoft Sentinel Workspace (soc-lab-workspace)                        |
|            ├─► SecurityEvent (EventID 4624, 4688)                                 |
|            ├─► SigninLogs (Entra ID)                                              |
|            └─► SecurityIncident (Ingested Alerts & Analytics)                     |
+-----------------------------------------------------------------------------------+
