Markdown
# 🛡️ End-to-End Windows Attack Simulation, Detection & SOC Investigation Lab
> **Atomic Red Team ➔ Sysmon ➔ Defender XDR ➔ Azure Arc / AMA ➔ Microsoft Sentinel**

---

## 📌 Project Overview
This repository documents the architecture, simulation, telemetry pipeline, and Security Operations Center (SOC) investigation workflow for an enterprise-grade Windows security monitoring lab.

By executing threat simulations using **Atomic Red Team** mapped to the **MITRE ATT&CK Framework**, security telemetry is generated on a target endpoint (**Windows 11 VM** with **Sysmon64** & **Microsoft Defender**) and streamed into **Microsoft Sentinel** (SIEM) and **Microsoft Defender XDR** for KQL threat hunting, incident triage, and root cause analysis.

---

## 🎨 System Architecture & Telemetry Pipeline
```text
+-----------------------------------------------------------------------------------+
|                            ATTACK SIMULATION LAYER                                |
|                                                                                   |
|  [ Kali Linux VM ] ------------- (RDP / T1021.001) ----------+                    |
|                                                              |                    |
|  [ Windows 11 VM ]                                           |                    |
|     `---> Invoke-AtomicTest (Execution, Persistence, UAC, etc.)|                  |
+-----------------------------------|-------------------------------+               |
                                    |                                               |
                                    v                                               |
+-----------------------------------------------------------------------------------+
|                            ENDPOINT TELEMETRY LAYER                               |
|                                                                                   |
|  [ Windows 11 VM ]                                                                |
|     |---> Sysmon (Event ID 1, 13, 104) ---> (Local EVTX Log)                      |
|     `---> Defender for Endpoint Sensor ---> (DeviceProcessEvents)                 |
+-----------------------------------|-------------------------------+               |
                                    |                                               |
                                    v                                               |
+-----------------------------------------------------------------------------------+
|                            CLOUD & SIEM INGESTION LAYER                           |
|                                                                                   |
|  [ Azure Arc / DCR Pipeline ]                                                     |
|     |                                                                             |
|     |---> Microsoft Defender XDR (security.microsoft.com)                         |
|     |     `---> Incident Triage & Alerts                                          |
|     |                                                                             |
|     `---> Microsoft Sentinel Workspace (soc-lab-workspace)                        |
|           |---> SecurityEvent (EventID 4624, 4688)                                |
|           |---> SigninLogs (Entra ID)                                             |
|           `---> SecurityIncident (Ingested Alerts & Analytics)                    |
+-----------------------------------------------------------------------------------+
```
