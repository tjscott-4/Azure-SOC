# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/user-attachments/assets/b392d7d9-e752-4c59-bd88-91fdf0ad5071)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/61e9676e-9718-4ef4-9ee6-97e90843fbea)

## Architecture After Hardening / Security Controls
![image](https://github.com/user-attachments/assets/cff6cb79-80d4-42b0-b8e6-5aa69f73355d)


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
![(before) nsg-malicious-allowed-in](https://github.com/user-attachments/assets/d7ed9f3a-7d8d-40be-be51-808d87170b72)
<br>
![(before) linux-ssh-auth-fail](https://github.com/user-attachments/assets/d880d6ff-ec00-4938-9b67-1948d3c8124b)
<br>
![(before) windows-rdp-auth-fail](https://github.com/user-attachments/assets/3858d99c-1d3e-4325-8cf7-87c20e4b08d2)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-11-02 17:00:54
Stop Time 2024-11-03 17:00:54

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 5652
| Syslog                   | 3586
| SecurityAlert            | 68
| SecurityIncident         | 69
| AzureNetworkAnalytics_CL | 2437

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-11-07 02:11:20
Stop Time	2024-11-08 02:11:20

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 371
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
