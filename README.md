# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/1ece8640-13c8-4201-9123-80b33c4e3484)


## Introduction

In this project, I've developed a compact honeynet within the Microsoft Azure framework. This honeynet serves as a controlled environment to simulate potential cyber threats. It integrates with various sources to gather logs, channeling them into a dedicated Log Analytics workspace. Microsoft Sentinel, our chosen tool, leverages these logs to construct visual attack maps, activate alerts in response to potential threats, and establish a structured approach to handling security incidents.

To evaluate the effectiveness of our security measures, I've conducted a comparative analysis. Initially, I measured key security metrics over a 24-hour period within an intentionally vulnerable environment. Subsequently, after implementing a series of security controls to bolster the system's resilience, I conducted another 24-hour metric assessment. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/9c3fb2f0-7c4a-4730-8310-e12cbbb5bca6)


## Architecture After Hardening / Security Controls
![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/7ce342a5-6b08-467a-923e-3d49dcbfd860)


The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

"BEFORE" metrics: All resources were deployed in a configuration that left them openly accessible via the internet. The Virtual Machines were configured with both their Network Security Groups and internal firewalls set to permissive settings. Additionally, all other deployed resources featured public endpoints that were visible to the internet, rendering the incorporation of Private Endpoints unnecessary.

"AFTER" metrics: Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
NSG Allowed Inbound Malicious Flows ![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/280bb627-2222-4bdd-85bc-5257b43026cd)<br>
Linux Syslog Auth Failures ![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/5aeb3ba4-302b-4223-a04f-a6d1a59b9f0d)<br>
Windows RDP/SMB Auth Failures ![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/76d709e5-9410-4068-b470-78ee1fd6b38b)
<br>
MSSQL-Auth-Fail ![image](https://github.com/nigelamoss/Azure-SOC/assets/91230399/92972fdf-5d39-4955-ae24-13294b2ddd39)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-08-29 14:40:42
Stop Time 2023-08-30 14:40:42

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 574147
| Syslog                   | 1463
| SecurityAlert            | 6
| SecurityIncident         | 207
| AzureNetworkAnalytics_CL | 2599

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
