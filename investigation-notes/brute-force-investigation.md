# 🔍 Brute Force Login Attack — Incident Investigation Report

**Date:** June 13, 2026  
**Analyst:** Kevin  
**Environment:** Microsoft Sentinel SOC Lab (Azure)  
**Severity:** Medium  
**Status:** ✅ Investigated — Lab Simulation Confirmed  

---

## 1. Incident Summary

Multiple failed login attempts were detected against the Windows VM `SOC-WIN10` in the Azure SOC Lab environment. The activity matches the pattern of a **brute-force password attack** targeting the local user account `testuser`.

This was a **controlled simulation** performed to verify the end-to-end SOC detection pipeline: Windows VM → Azure Monitor Agent → Log Analytics Workspace → Microsoft Sentinel.

---

## 2. Affected System

| Field | Value |
|-------|-------|
| Hostname | SOC-WIN10 |
| OS | Windows 10 Enterprise |
| Region | Southeast Asia (Singapore) |
| Resource Group | SOC-Lab |
| Public IP | 20.24.147.251 |
| VM Size | Standard B2as v2 (2 vCPU / 8 GiB) |

---

## 3. Detection

### Trigger
**Windows Security Event ID 4625** — "An account failed to log on"

### Detection Source
Microsoft Sentinel → Log Analytics Workspace → SecurityEvent table

### First Observed
6/12/2026 at 7:30 PM (local Event Viewer)  
6/13/2026 at 6:05 AM (Sentinel — after AMA forwarding)

### KQL Query Used for Detection

```kql
SecurityEvent
| where EventID == 4625
| where TargetUserName == "testuser"
| project TimeGenerated, TargetUserName, Computer, EventID, Activity, IpAddress, LogonTypeName
| order by TimeGenerated desc
```

---

## 4. Attack Simulation Details

### What Was Done

1. Created a local Windows user account named `testuser` using `lusrmgr.msc`
2. Ran the `runas /user:testuser cmd` command from the command prompt
3. Entered incorrect passwords repeatedly (10+ times) to generate failed login events
4. This produced **Event ID 4625** entries in the Windows Security log

### Evidence — Local Event Viewer

- **Total 4625 events captured:** 35,517
- **Log name:** Security
- **Source:** Microsoft Windows security auditing
- **Keywords:** Audit Failure
- **Task Category:** Logon
- **Computer:** SOC-WIN10
- **Date range:** 6/12/2026 – 6/13/2026

---

## 5. Sentinel Investigation Results

### Query 1 — General Failed Login Scan

```kql
SecurityEvent
| where EventID == 4625
```

**Results:** 16 events returned (last 1 hour window), all from `SOC-WIN10`, accounts including `\Administrator` and `testuser`.

### Query 2 — Targeted User Investigation

```kql
SecurityEvent
| where EventID == 4625
| where TargetUserName == "testuser"
| project TimeGenerated, TargetUserName, Computer, EventID, Activity, IpAddress, LogonTypeName
| order by TimeGenerated desc
```

**Results:**

| TimeGenerated [UTC] | TargetUserName | Computer | EventID | Activity |
|---------------------|---------------|----------|---------|----------|
| 6/13/2026 6:05:40 AM | testuser | SOC-WIN10 | 4625 | An account failed to log on |
| 6/13/2026 6:05:36 AM | testuser | SOC-WIN10 | 4625 | An account failed to log on |
| 6/13/2026 6:05:25 AM | testuser | SOC-WIN10 | 4625 | An account failed to log on |
| 6/13/2026 6:05:12 AM | testuser | SOC-WIN10 | 4625 | An account failed to log on |

**Total:** 4 confirmed events targeting `testuser`

---

## 6. Sentinel Dashboard Observations

At time of investigation (6/13/2026 at 3:37 PM):

| Metric | Value |
|--------|-------|
| Incidents (last 24h) | 0 (no alert rule configured yet) |
| Active Data Connectors | 2 |
| Analytics Rules | 1 Enabled |
| Data received | Multiple spikes visible in chart |

> **Note:** No automated incident was triggered because a custom Analytics Rule targeting Event ID 4625 was not yet configured. This is the next step — creating a Scheduled Analytics Rule to auto-alert on brute force patterns.

---

## 7. Root Cause Analysis

| Field | Detail |
|-------|--------|
| Attack Type | Brute Force — Repeated failed login attempts |
| Target Account | testuser (local account on SOC-WIN10) |
| Attack Source | Internal simulation (runas command) |
| Log Detected | Windows Security Event ID 4625 |
| Log Forwarded | Yes — via Azure Monitor Agent v1.43.0 |
| Sentinel Detected | Yes — visible in SecurityEvent table via KQL |

---

## 8. Recommended Response Actions

In a real production environment, the following actions would be taken:

1. **Lock the targeted account** — Prevent further login attempts
2. **Identify source IP** — Check IpAddress field in Sentinel logs
3. **Review all activity from that IP** — Look for lateral movement
4. **Reset credentials** — Force password change for affected account
5. **Enable Account Lockout Policy** — Lock after N failed attempts (Group Policy)
6. **Enable MFA** — Multi-factor authentication on all accounts
7. **Create Sentinel Alert Rule** — Auto-trigger incidents on 5+ failures in 5 minutes
8. **Monitor for persistence** — Check for new accounts, scheduled tasks, registry changes

---

## 9. Detection Pipeline — Confirmed Working

```
✅ Windows VM (SOC-WIN10)
      ↓  Generated Event ID 4625 (35,517 events locally)
✅ Azure Monitor Agent (v1.43.0)
      ↓  Forwarded logs to cloud (5-15 min delay)
✅ Log Analytics Workspace (SOC-WORKSPACES)
      ↓  Stored and indexed security events
✅ Microsoft Sentinel
      ↓  Events visible in SecurityEvent table
✅ KQL Investigation
      ↓  Successfully queried and identified testuser attacks
✅ SOC Analyst
      ↓  Incident documented and response actions identified
```

---

## 10. Lessons Learned

- The Azure Monitor Agent forwarded logs with approximately a **5–15 minute delay** — expected behavior
- **35,517 events** were generated locally but only a subset appeared in Sentinel (based on the time window queried)
- Without a configured **Analytics Rule**, Sentinel does not auto-create incidents — this must be set up separately
- KQL is powerful for **targeted investigation** — filtering by `TargetUserName` instantly isolated the attack

---

*Report prepared as part of Microsoft Sentinel SOC Lab — June 2026*
