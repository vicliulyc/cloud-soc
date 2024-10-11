# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/user-attachments/assets/025385f1-8f71-4d27-aa18-42bd19c91bae)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/10f3ae34-aacd-4ee8-a214-219a0b9d6fb1)


## Architecture After Hardening / Security Controls
![image](https://github.com/user-attachments/assets/8c33310e-204a-4445-a3be-53d9737cb536)


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
<img width="964" alt="BEFOREFAILEDmssql" src="https://github.com/user-attachments/assets/62933ae7-a166-4812-b1e6-0c695d510f04">
<img width="955" alt="BEFOREFAILEDLINUX" src="https://github.com/user-attachments/assets/f8a9203d-1d75-4b50-a8a5-3fb819f5176e">
<img width="957" alt="BEFOREnsgALLOWEDIN" src="https://github.com/user-attachments/assets/14198634-0330-49ca-b682-5ba0e1d58234">
<img width="950" alt="BEFOREWINDOWSFAIL" src="https://github.com/user-attachments/assets/d600d4d0-ef9d-4286-ba5d-9400b3bbe108">



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-10-08 7:56:18 
Stop Time 2024-10-09 7:56:18

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 99156
| Syslog                   | 12168
| SecurityAlert            | 6
| SecurityIncident         | 254
| AzureNetworkAnalytics_CL | 2138

## Attack Maps After Hardening / Security Controls
<img width="919" alt="AFTERMSSQLFAIL" src="https://github.com/user-attachments/assets/902f5ed1-684d-4082-ba85-43e97b860f38">
<img width="942" alt="AFTERLINUXFAIL" src="https://github.com/user-attachments/assets/2c44b841-ad58-4f11-99bf-2d4baa27123b">
<img width="933" alt="AFTERNSGALLOWED" src="https://github.com/user-attachments/assets/22a4d4f3-01a1-4f2b-8196-5dc4321431a6">
<img width="917" alt="AFTERWINDOWFAIL" src="https://github.com/user-attachments/assets/72ea697e-6595-4ff8-b52f-d46f810b57c9">



## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:
Start Time 2024-10-09 7:56:34
Stop Time	2024-10-09 7:56:34

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 63157
| Syslog                   | 11772
| SecurityAlert            | 1
| SecurityIncident         | 201
| AzureNetworkAnalytics_CL | 3313

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
