# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/user-attachments/assets/025385f1-8f71-4d27-aa18-42bd19c91bae)

## Introduction

The primary goal of this project is to build a honeynet that enables the analysis of real-time cyberattacks, facilitates incident response, and allows for the investigation of event alerts. This will provide insight into attackers' intentions, tactics, techniques, and procedures (TTPs). The secondary goal is to enhance the security of the environment by reconfiguring the firewall and Network Security Groups (NSGs), implementing Azure Private Links, and applying regulatory compliance measures, including **NIST 800-53** and recommendations from **Microsoft Defender for Cloud**.

Over two 24-hour periods, I measured key security metrics in both an insecure and hardened environment, showing the impact of applying security controls. The metrics we’ll review include:

- **SecurityEvent** (Windows Event Logs)
- **Syslog** (Linux Event Logs)
- **SecurityAlert** (Log Analytics Alerts Triggered)
- **SecurityIncident** (Incidents created by Sentinel)
- **AzureNetworkAnalytics_CL** (Malicious Flows detected in the honeynet)


### Technologies and Requirements
- **Azure Virtual Network (VNet)** for network segmentation
- **Azure Network Security Groups (NSG)** for controlling inbound and outbound traffic
- **Virtual Machines**: 2 Windows VMs and 1 Linux VM for diverse OS environments
- **Log Analytics Workspace** with **Kusto Query Language (KQL)** for advanced log querying and analysis
- **Azure Key Vault** for secure management of secrets and credentials
- **Azure Storage Account** for reliable data storage solutions
- **Microsoft Sentinel** as the **SIEM** for real-time threat detection and response
- **Microsoft Defender for Cloud** to safeguard cloud resources
- **Windows Remote Desktop** for secure remote system access
- **Command Line Interface (CLI)** for efficient system management
- **PowerShell** for automation and configuration tasks
- **NIST SP 800-53 Revision 5** for implementing security controls
- **NIST SP 800-61 Revision 2** for incident response best practices and guidance

## Architecture Before Hardening / Security Controls
![image](https://github.com/user-attachments/assets/10f3ae34-aacd-4ee8-a214-219a0b9d6fb1)


## Architecture After Hardening / Security Controls
![image](https://github.com/user-attachments/assets/8c33310e-204a-4445-a3be-53d9737cb536)



For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
<img width="964" alt="BEFOREFAILEDmssql" src="https://github.com/user-attachments/assets/62933ae7-a166-4812-b1e6-0c695d510f04"> 
This attack map shows all the attempts threat actors trying to access the Microsoft SQL Database Server within the Windows virtual machine


<img width="955" alt="BEFOREFAILEDLINUX" src="https://github.com/user-attachments/assets/f8a9203d-1d75-4b50-a8a5-3fb819f5176e"> This attack map shows all the attempts threat actors trying to access the Linux virtual machine via SSH


<img width="957" alt="BEFOREnsgALLOWEDIN" src="https://github.com/user-attachments/assets/14198634-0330-49ca-b682-5ba0e1d58234"> This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound


<img width="950" alt="BEFOREWINDOWSFAIL" src="https://github.com/user-attachments/assets/d600d4d0-ef9d-4286-ba5d-9400b3bbe108">
This attack map shows all the attempts threat actors trying to access the Windows virtual machine via RDP

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







## For the "AFTER" stage of the project, the environment was hardened by implementing security controls in compliance with **NIST SP 800-53 Rev4 SC-7(3) Access Points**. These measures included:

- **Network Security Groups (NSGs)**: The NSGs were strengthened by blocking all inbound and outbound traffic except for specific public IP addresses requiring access to the virtual machines. This ensured that only authorized, trusted sources could reach the VMs, minimizing exposure to potential threats.

- **Built-in Firewalls**: Azure’s built-in firewalls were configured on each virtual machine to prevent unauthorized access and defend against malicious traffic. Firewall rules were fine-tuned to align with the specific services and roles of each VM, reducing the attack surface available to bad actors.

- **Private Endpoints**: Public endpoints for Azure Key Vault and Storage Containers were replaced with **private endpoints**, ensuring these sensitive resources were restricted to the virtual network and not exposed to the public internet.

- **Subnetting**: To further isolate and protect critical resources, a dedicated subnet was created for Azure Key Vault and Storage Containers. This added an extra layer of security by segregating traffic and controlling access to these endpoints.

## Attack Maps After Hardening / Security Controls
<img width="919" alt="AFTERMSSQLFAIL" src="https://github.com/user-attachments/assets/902f5ed1-684d-4082-ba85-43e97b860f38"> This attack map shows the reduction of traffic allowed by Microsoft SQL Database Server after applying NIST 800-53 controls


<img width="942" alt="AFTERLINUXFAIL" src="https://github.com/user-attachments/assets/2c44b841-ad58-4f11-99bf-2d4baa27123b"> This attack map shows the reduction of attempts threat actors trying to access the Linux virtual machine after applying NIST 800-53 controls


<img width="933" alt="AFTERNSGALLOWED" src="https://github.com/user-attachments/assets/22a4d4f3-01a1-4f2b-8196-5dc4321431a6"> This attack map shows the reduction of traffic allowed by an NSG after applying NIST 800-53 controls
<img width="917" alt="AFTERWINDOWFAIL" src="https://github.com/user-attachments/assets/72ea697e-6595-4ff8-b52f-d46f810b57c9"> This attack map shows the reduction of attempts threat actors trying to access the Windows virtual machine after applying NIST 800-53 controls



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

In conclusion, this Azure Sentinel Honeypot Homelab provided a hands-on opportunity to explore real-world cyberattacks, enhance incident response skills, and gain deeper insights into attackers' tactics, techniques, and procedures. Through the implementation of advanced security measures—such as Network Security Groups, built-in firewalls, private endpoints, and subnetting—we transformed an initially insecure environment into a fortified system compliant with industry standards like **NIST SP 800-53**.

Reflecting on the experience, the lab not only showcased the importance of proactive defense strategies but also reinforced the value of continuous monitoring and log analysis in detecting and mitigating potential threats. This project exemplifies how critical the right security controls are in defending cloud environments, making it a vital learning experience for any cybersecurity professional.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
