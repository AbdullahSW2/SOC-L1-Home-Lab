# SOC Level 1 Home Lab – Setup Documentation

## 1. Overview

This document describes the infrastructure and configuration of a practical Security Operations Center (SOC) Level 1 home lab.

The environment uses Oracle VirtualBox to run three virtual machines connected through a shared NAT Network.

The primary purpose of the lab is to practice security monitoring, Windows event log analysis, threat detection, and incident investigation using Splunk Enterprise.

**Project Status:** Operational – Documentation and Investigation Ongoing

## 2. Lab Components

| Component | Role |
|---|---|
| Oracle VirtualBox | Virtualization platform |
| Kali Linux | Authorized security testing and attack simulation |
| Windows 10 | Target endpoint and Windows event generation |
| Ubuntu Linux | Security monitoring server |
| Splunk Enterprise 10.6.1.1 | Security log search and analysis |

## 3. Virtual Machine Configuration

Three virtual machines were configured in Oracle VirtualBox.

### Kali Linux

- Operating System: Kali Linux
- IP Address: `192.168.100.4`
- Network Adapter 1: NAT Network
- Network Adapter 2: Disabled
- Network Name: SOC Lab
- Role: Controlled security testing and attack simulation

### Windows 10

- Operating System: Windows 10
- IP Address: `192.168.100.50`
- Network Adapter 1: NAT Network
- Network Adapter 2: Disabled
- Network Name: SOC Lab
- Role: Target endpoint for security event generation

### Ubuntu Linux

- Operating System: Ubuntu Linux
- IP Address: `192.168.100.5`
- Network Adapter 1: NAT Network
- Network Adapter 2: Disabled
- Network Name: SOC Lab
- Role: Splunk server and security event analysis

## 4. Network Configuration

| Setting | Value |
|---|---|
| Network Type | NAT Network |
| Network Name | SOC Lab |
| Subnet | 192.168.100.0/24 |
| Virtual Machines | 3 |
| Connectivity | Verified |

All virtual machines use the same NAT Network.

This configuration enables communication between lab systems.

External connectivity depends on the VirtualBox NAT Network configuration and the host's network connection.

## 5. Connectivity Verification

ICMP connectivity was tested between all three virtual machines.

| Source | Destination | Result |
|---|---|---|
| Kali Linux | Windows 10 | PASS |
| Kali Linux | Ubuntu | PASS |
| Windows 10 | Kali Linux | PASS |
| Windows 10 | Ubuntu | PASS |
| Ubuntu | Kali Linux | PASS |
| Ubuntu | Windows 10 | PASS |

**Result: 6/6 successful connectivity tests.**

All documented tests completed with 0% packet loss.

## 6. Splunk Environment

Splunk Enterprise 10.6.1.1 is running on the Ubuntu virtual machine.

The Splunk Web interface is accessible locally through:

`http://127.0.0.1:8000`

The environment includes an index named:

`windows_logs`

Windows authentication events have been successfully queried in Splunk.

### Verified Windows Events

- Event ID 4624 – Successful Logon
- Event ID 4625 – Failed Logon

The exact Windows log forwarding mechanism has not yet been documented or independently verified.

## 7. Log Monitoring Workflow

The lab supports the following analysis workflow:

1. Generate Windows security activity within the authorized lab.
2. Collect Windows authentication events.
3. Search and analyze events in Splunk.
4. Identify source IP addresses and affected accounts.
5. Compare successful and failed authentication events.
6. Document investigation findings.

## 8. Verification Evidence

Connectivity and security monitoring screenshots are available in the repository's `screenshots/` directory.

Additional investigation reports are available in `investigations/`.

## 9. Security Considerations

- Testing is restricted to authorized lab systems.
- No credentials or sensitive authentication tokens are published.
- Virtual machine disk images and configuration secrets are excluded from GitHub.
- The lab network should not be treated as fully isolated from external networks solely because it uses NAT Network.
- External exposure and internet access should be reviewed before performing security simulations.

## 10. Future Improvements

- Document the exact Windows log forwarding method.
- Verify log ingestion configuration.
- Develop additional SPL detection searches.
- Create Splunk dashboards and alert rules.
- Expand incident investigation scenarios.
- Document reproducible installation and configuration steps.

## 11. Disclaimer

This project is intended for educational cybersecurity research, authorized security testing, and defensive SOC training only.
