# 🛡️ Microsoft Sentinel SOC Lab
### Cloud-Based Security Operations Center | Brute Force Detection & KQL Investigation

![Azure](https://img.shields.io/badge/Microsoft_Azure-0078D4?style=for-the-badge&logo=microsoft-azure&logoColor=white)
![Sentinel](https://img.shields.io/badge/Microsoft_Sentinel-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![Windows](https://img.shields.io/badge/Windows_10-0078D4?style=for-the-badge&logo=windows&logoColor=white)
![KQL](https://img.shields.io/badge/KQL-Query_Language-green?style=for-the-badge)

---

## 📋 Project Overview

This project simulates a real-world **Security Operations Center (SOC)** environment built entirely on Microsoft Azure. The goal was to deploy a cloud-based SIEM, collect Windows security event logs from a live virtual machine, simulate a brute-force login attack, and investigate the attack using KQL queries inside Microsoft Sentinel.

This lab demonstrates the **end-to-end SOC analyst workflow** — from infrastructure setup to threat detection and incident investigation.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    AZURE SOC LAB                        │
│                                                         │
│   🖥️  Windows VM (SOC-WIN10)                           │
│       │  Windows 10 Enterprise                          │
│       │  Status: Running | Region: Southeast Asia       │
│       │                                                 │
│       ▼  (Generates Event ID 4625 — Failed Logins)     │
│                                                         │
│   📡  Azure Monitor Agent (AMA)                        │
│       │  Version: 1.43.0 | Status: Provisioning OK     │
│       │                                                 │
│       ▼  (Forwards logs to cloud)                      │
│                                                         │
│   📊  Log Analytics Workspace (SOC-WORKSPACES)         │
│       │  Location: Southeast Asia | Status: Active      │
│       │                                                 │
│       ▼  (Stores & indexes all security logs)          │
│                                                         │
│   🛡️  Microsoft Sentinel                               │
│       │  Workspace: soc-workspaces                     │
│       │  Active Connectors: 2 | Analytics Rules: 1     │
│       │                                                 │
│       ▼  (Detects threats, creates incidents)          │
│                                                         │
│   🔍  KQL Investigation                                │
│       │  SecurityEvent | where EventID == 4625         │
│       │                                                 │
│       ▼                                                │
│                                                         │
│   🧑‍💻  SOC Analyst (You)                               │
└─────────────────────────────────────────────────────────┘
```

---

## 🧰 Tools & Technologies

| Category | Tool | Purpose |
|----------|------|---------|
| ☁️ Cloud Platform | Microsoft Azure | Infrastructure hosting |
| 🛡️ SIEM | Microsoft Sentinel | Threat detection & incident management |
| 📊 Log Storage | Log Analytics Workspace | Central log database |
| 📡 Log Collection | Azure Monitor Agent (AMA) | Forwards VM logs to cloud |
| 🖥️ Endpoint | Windows 10 Enterprise VM | Attack target / log source |
| 🔍 Query Language | KQL (Kusto Query Language) | Log investigation & threat hunting |
| 🔐 Attack Simulation | Event ID 4625 | Failed login / brute-force simulation |

---

## ⚙️ Lab Configuration

| Resource | Name | Region | Status |
|----------|------|--------|--------|
| Resource Group | SOC-Lab | Southeast Asia | ✅ Active |
| Log Analytics Workspace | SOC-WORKSPACES | Southeast Asia | ✅ Active |
| Windows VM | SOC-WIN10 | Southeast Asia | ✅ Running |
| VM Size | Standard B2as v2 | 2 vCPUs / 8 GiB RAM | ✅ |
| OS | Windows 10 Enterprise | - | ✅ |
| Monitor Agent | AzureMonitorWindowsAgent | v1.43.0 | ✅ Provisioning succeeded |
| Sentinel Workspace | soc-workspaces | Southeast Asia | ✅ Active |

---

## 🎯 Lab Scenario — Brute Force Attack Simulation

### Step 1 — Created Attack Target Account
Inside the Windows VM, a local user account was created to act as the attack target:

```
Username: testuser
Tool: lusrmgr.msc (Local Users and Groups)
```

### Step 2 — Simulated Brute Force Login Attempts
Multiple failed login attempts were generated using the Windows `runas` command with incorrect passwords:

```cmd
runas /user:testuser cmd
# Enter wrong password when prompted
# Repeat 10+ times
```

This generated **Windows Security Event ID 4625** (An account failed to log on) in the local Security event log.

### Step 3 — Verified Locally in Event Viewer
Opened Event Viewer → Windows Logs → Security → Filtered by Event ID 4625.

Result: **35,517 Audit Failure events** captured, confirming successful attack simulation.

### Step 4 — Azure Monitor Agent Forwarded Logs
The AMA agent (installed on SOC-WIN10) automatically forwarded the security logs to the Log Analytics Workspace within ~5–15 minutes.

### Step 5 — Detected Attack in Microsoft Sentinel
Ran KQL queries in Sentinel Logs to detect and investigate the brute-force activity.

---

## 🔍 KQL Detection & Investigation

### Basic Detection — All Failed Logins
```kql
SecurityEvent
| where EventID == 4625
```
**Result:** 16 events returned from SOC-WIN10 (sample from last hour)

### Targeted Investigation — Specific User
```kql
SecurityEvent
| where EventID == 4625
| where TargetUserName == "testuser"
| project TimeGenerated, TargetUserName, Computer, EventID, Activity, IpAddress, LogonTypeName
| order by TimeGenerated desc
```
**Result:** 4 confirmed failed login events targeting `testuser` on `SOC-WIN10`

### Brute Force Summary — Count by Account
```kql
SecurityEvent
| where EventID == 4625
| summarize FailedAttempts = count() by TargetUserName
| order by FailedAttempts desc
```

See full KQL files in the [`KQL-queries/`](./KQL-queries/) folder.

---

## 📸 Screenshots

| # | File | What It Shows |
|---|------|---------------|
| 01 | [Azure Portal](./screenshots/01-Azure-Portal.png) | Azure environment logged in |
| 02 | [Resource Group](./screenshots/02-Resource-Group.png) | SOC-Lab resource group created |
| 03 | [Workspace](./screenshots/03-Workspace.png) | Log Analytics Workspace active |
| 04 | [Sentinel](./screenshots/04-Sentinel.png) | Microsoft Sentinel enabled |
| 05 | [VM Running](./screenshots/05-VM-Created___Running.png) | SOC-WIN10 status: Running |
| 06 | [RDP Connected](./screenshots/06-RDP-Connected.png) | RDP session into the VM |
| 07 | [Agent Connected](./screenshots/07-Agent-Connected.png) | AMA provisioning succeeded |
| 08 | [User Creation](./screenshots/08-User-Creation.png) | testuser created in VM |
| 09 | [Failed Logins](./screenshots/09-Failed-Login-Attempts.png) | Event ID 4625 in Event Viewer |
| 10 | [KQL Detection](./screenshots/10-KQL-Failed-Logins_png.PNG) | Sentinel KQL query results |
| 11 | [Sentinel Logs 4625](./screenshots/11-Sentinel-Logs-4625.png) | Targeted testuser detection |
| 12 | [Sentinel Dashboard](./screenshots/12-Sentinel-Dashboard.png) | Sentinel overview dashboard |

---

## 📁 Repository Structure

```
SOC-Lab-Microsoft-Sentinel/
│
├── README.md                          ← You are here
│
├── screenshots/                       ← All lab evidence screenshots
│   ├── 01-Azure-Portal.png
│   ├── 02-Resource-Group.png
│   ├── 03-Workspace.png
│   ├── 04-Sentinel.png
│   ├── 05-VM-Created___Running.png
│   ├── 06-RDP-Connected.png
│   ├── 07-Agent-Connected.png
│   ├── 08-User-Creation.png
│   ├── 09-Failed-Login-Attempts.png
│   ├── 10-KQL-Failed-Logins_png.PNG
│   ├── 11-Sentinel-Logs-4625.png
│   └── 12-Sentinel-Dashboard.png
│
├── KQL-queries/                       ← All KQL detection queries
│   ├── failed-login-detection.kql
│   ├── targeted-user-investigation.kql
│   └── brute-force-summary.kql
│
└── investigation-notes/               ← Incident report & findings
    └── brute-force-investigation.md
```

---

## ✅ Skills Demonstrated

- ✔️ **Microsoft Azure** — Resource groups, VMs, networking
- ✔️ **Microsoft Sentinel** — SIEM setup, data connectors, analytics rules
- ✔️ **Log Analytics Workspace** — Log ingestion, retention, querying
- ✔️ **Azure Monitor Agent (AMA)** — Agent deployment and configuration
- ✔️ **Windows Security Events** — Event ID 4625, Event Viewer, Security logs
- ✔️ **KQL (Kusto Query Language)** — Threat hunting, filtering, summarizing
- ✔️ **SOC Analyst Workflow** — Detection → Investigation → Response
- ✔️ **Brute Force Attack Simulation** — Ethical attack reproduction in lab
- ✔️ **Incident Documentation** — Professional security report writing

---

## 🧹 Cleanup (Save Azure Credits)

When done with the lab, delete all resources to stop billing:

```
Azure Portal → Resource Groups → SOC-Lab → Delete resource group
```

This removes the VM, workspace, Sentinel, and all associated resources at once.

---

*Built by Kevin | Microsoft Sentinel SOC Lab | June 2026*
