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
🔄 The 3-Tier Investigation Methodology
Every executed scenario follows a strict three-tier verification methodology to evaluate visibility across local endpoint logs, EDR portals, and SIEM data lakes:

+-------------------------+      +-------------------------+      +-------------------------+
|   STAGE A: Sysmon EVTX  | ───► |  STAGE B: Sentinel KQL  | ───► |  STAGE C: Defender XDR  |
|  (Host Ground Truth)    | ...  |  (SIEM Data Aggregation)| ───► |  (EDR Alert Context)    |
+-------------------------+      +-------------------------+      +-------------------------+

Stage A — Host Ground Truth (Sysmon / PowerShell): Validate execution directly at the kernel/process level on the endpoint using local event logs before any network transport.

Stage B — SIEM Data Ingestion (Microsoft Sentinel): Query central log tables via KQL to verify Azure Monitor Agent (AMA) ingestion, schema mapping, and parser performance.

Stage C — EDR & Alert Context (Defender XDR): Evaluate telemetry enrichment, behavioral correlation, timeline aggregation, and incident generation in the Defender portal.

