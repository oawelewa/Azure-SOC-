# Building a SOC + Honeynet in Azure (Live Traffic)
![Cybersecurity-Azure-SOC](https://github.com/oawelewa/Azure-SOC-/assets/132318130/1c6ce36b-aed9-43bd-89e3-453e6e22158e)
## Introduction

In this project, I set up a mini honeynet in Azure and ingested log sources from various resources into a Log Analytics workspace. Microsoft Sentinel then used this workspace to create attack maps, trigger alerts, and generate incidents. I measured security metrics in the unsecured environment for 24 hours, applied security controls to harden the environment, measured metrics for another 24 hours, and presented the results below. The metrics we will examine are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/oawelewa/Azure-SOC-/assets/132318130/4722260f-1829-4de7-a23e-ce3610f3501f)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/oawelewa/Azure-SOC-/assets/132318130/077bf2a9-d73d-4a51-99db-450fe0ca1ebd)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/oawelewa/Azure-SOC-/assets/132318130/7c297168-a4c0-4afe-8ee6-2ac044e3e834)<br>
![Linux Syslog Auth Failures](https://github.com/oawelewa/Azure-SOC-/assets/132318130/8d317b64-c09b-4125-8a3b-1b710084a45a)<br>
![Windows RDP/SMB Auth Failures](https://github.com/oawelewa/Azure-SOC-/assets/132318130/8e135cd1-30aa-40ec-a009-5c7505fc9522)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-03-28 01:44:39
Stop Time 2024-03-29 01:44:39

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 37610
| Syslog                   | 3256
| SecurityAlert            | 9
| SecurityIncident         | 269
| AzureNetworkAnalytics_CL | 2521

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-04-25 12:20:26
Stop Time	2024-04-26 12:20:26

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 16462
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was built in Microsoft Azure, with log sources integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Metrics were measured in the insecure environment before applying security controls, and then again after implementing security measures. The number of security events and incidents significantly decreased after the security controls were applied, demonstrating their effectiveness.

It is important to note that if the network resources were heavily utilized by regular users, more security events and alerts might have been generated within the 24-hour period following the implementation of the security controls.
