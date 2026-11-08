# Windows Authentication Detection – Splunk SPL

## Overview

This document contains Splunk Search Processing Language (SPL) queries used to investigate Windows authentication events in the SOC Level 1 Home Lab.

**SIEM:** Splunk Enterprise  
**Index:** `windows_logs`  
**Target:** Windows 10  
**Relevant Event IDs:**
- 4624 – Successful Logon
- 4625 – Failed Logon

## Detection 01 – Failed Login Attempts

**Objective:** Identify failed Windows authentication attempts grouped by account and source IP.

```spl
index="windows_logs" EventCode=4625
| stats count by Account_Name, Source_Network_Address
| sort -count
```

**Analysis:** Helps identify accounts receiving repeated failed authentication attempts.

## Detection 02 – Multiple Account Authentication Activity

**Objective:** Count distinct account names associated with each source IP.

```spl
index="windows_logs" (EventCode=4625 OR EventCode=4624)
| stats dc(Account_Name) as unique_users count by Source_Network_Address
| sort -count
```

**Analysis:** Useful for identifying sources interacting with multiple accounts. Additional investigation is required to distinguish legitimate activity from potential password spraying or enumeration.

## Detection 03 – Successful vs Failed Logins

**Objective:** Compare successful and failed authentication events by account and source IP.

```spl
index="windows_logs" (EventCode=4625 OR EventCode=4624)
| stats count(eval(EventCode=4625)) as failures
        count(eval(EventCode=4624)) as success
        by Account_Name, Source_Network_Address
| sort -failures
```

**Analysis:** Supports authentication anomaly investigation and correlation of successful and failed logons.

## Detection 04 – Authentication Activity Timeline

**Objective:** Identify the first and last observed authentication events for each source IP.

```spl
index="windows_logs" (EventCode=4625 OR EventCode=4624)
| stats count min(_time) as first_seen max(_time) as last_seen by Source_Network_Address
| eval first_seen=strftime(first_seen,"%Y-%m-%d %H:%M:%S")
| eval last_seen=strftime(last_seen,"%Y-%m-%d %H:%M:%S")
| sort -count
```

**Analysis:** Helps identify when authentication activity occurred and which source IP addresses generated the most events.

## Observed Results

During the documented lab exercises:

- Windows authentication events were successfully retrieved from the `windows_logs` index.
- Multiple failed login events were identified.
- Source IP `192.168.100.4` appeared in authentication results.
- One search returned 23 authentication events associated with that source IP.
- Multiple account names were observed.

These observations were made across separate searches and time windows. They should not be interpreted as a single consolidated incident count.

## Investigation Notes

Authentication events alone do not prove malicious activity or a successful compromise. Event timing, logon type, source context, and supporting evidence must be reviewed before determining an incident classification.

## Evidence

Supporting screenshots are available in the repository's `screenshots/` directory.

## Status

Initial authentication detection queries documented. Further investigation and validation are ongoing.
