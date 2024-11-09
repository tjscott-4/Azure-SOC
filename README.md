# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/user-attachments/assets/b392d7d9-e752-4c59-bd88-91fdf0ad5071)


## Introduction

For this project, I set up a mini honeynet in Azure and configured it to gather log data from multiple resources into a Log Analytics workspace. Using Microsoft Sentinel, I was able to build attack maps, set up automated alerting, and generate incidents based on activity. Initially, I recorded key security metrics in an intentionally unsecured environment over a 24-hour period. Then, after applying security controls to strengthen the setup, I collected these metrics again for another 24 hours to compare the results. The findings are presented below:

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

In the "BEFORE" metrics phase, all resources were initially deployed and openly accessible from the internet. The Virtual Machines had unrestricted Network Security Groups and firewalls, leaving them fully exposed. Additionally, all other resources were configured with public endpoints, making them visible to external networks without any Private Endpoints in place.



In the "AFTER" metrics phase, the Network Security Groups were fortified to block all traffic except for connections from my admin workstation. Additionally, all other resources were secured using both their built-in firewalls and Private Endpoints, ensuring that access was restricted and protected from external exposure.

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

In this project, a mini honeynet was set up within Microsoft Azure, with log data collected into a Log Analytics workspace. Microsoft Sentinel was utilized to monitor these logs, triggering alerts and creating incidents based on suspicious activity. Security metrics were initially captured from the insecure environment, followed by a second round of measurements after security controls were applied. A notable decrease in security events and incidents was observed after the implementation of these controls, demonstrating their effectiveness in reducing vulnerabilities.

It should be noted that if the network resources had been under heavy use by regular users, the volume of security events and alerts might have been higher during the 24 hours after the security controls were applied.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
