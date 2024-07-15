# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/DoqN172.jpeg)

## Introduction / Objective

In this project, a mini honeynet was constructed within the Azure platform. The objective was to capture and analyze logs from several sources, subsequently consolidated within a Log Analytics workspace. Microsoft Sentinel was deployed to leverage these logs by developing attack maps, creating alert triggers, and incident generation. Azure Sentinel measured the metrics of an insecure environment over a 24-hour period. Following this phase, security controls were implemented to fortify the virtual environment. Lastly, another 24-hour metric measurement phase was conducted and the results obtained from these endeavors are presented below. The metrics analyzed were:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Technologies, Azure Components, and Regulations Employed

- Azure Virtual Network (VNet)
- Azure Network Security Groups (NSG)
- Virtual Machines (2 Windows VMs, 1 Linux VM)
- Log Analytics Workspace with Kusto Query Language (KQL) Queries
- Azure Key Vault for Secure Secrets Management
- Azure Storage Account for Data Storage
- Microsoft Sentinel for Security Information and Event Management (SIEM)
- Microsoft Defender for Cloud to Protect Cloud Resources
- Windows Remote Desktop for Remote Access
- Command Line Interface (CLI) for System Management
- PowerShell for Automation and Configuration Management
- <a href="https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final">NIST SP 800-53 Revision 5</a> for Security Controls
- <a href="https://www.nist.gov/privacy-framework/nist-sp-800-61">NIST SP 800-61 Revision 2</a> for Incident Handling Guidance


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/ThLNrOh.jpeg)

During the "BEFORE" stage of the project, <a href="https://github.com/MalikCyberDaily/Creating-Azure-Honeypot/tree/main">a virtual environment was deployed </a> and exposed to the public for malicious actors to discover. The intent for this stage was to attract bad actors and observe their attack patterns. To achieve this, A Windows virtual machine hosting a SQL database was deployed and a Linux server both had their network security groups (NSGs) had Allow All configured. To entice these attackers even further, a storage account and key vault were deployed with public endpoints which were visible on the open internet. In this stage, the unsecured environment was monitored by Microsoft Sentinel using logs aggregated by the Log Analytics workspace.

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/JqGxUx9.jpeg)

During the "AFTER" stage of the project, the environment was hardened and security controls were implemented in order to satisfy NIST SP 800-53 Rev4 SC-7(3) Access Points. These hardening tactics included:

- <b>Network Security Groups (NSGs):</b> NSGs were hardened by blocking all inbound and outbound traffic with the exception of designated public IP addresses that required access to the virtual machines. This ensured that only authorized traffic from a trusted source was allowed to access the virtual machines.

- <b>Built-in Firewalls:</b> Azure's built-in firewalls were configured on the virtual machines to restrict unauthorized access and protect the resources from malicious connections. This step involved fine-tuning the firewall rules based on the service and responsibilities of each VM which mitigated the attack surface bad actors had access to.

- <b>Private Endpoints:</b> To enhance the security of Azure Key Vault and Storage Containers, Public Endpoints were replaced with Private Endpoints. This ensured that access to these sensitive resources was limited to the virtual network and not the public internet.


## Attack Maps Before Hardening / Security Controls

<b>This attack map shows the traffic allowed by a Network Security Group with all traffic allowed inbound</b>
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1momEZw.png)<br>

<b> This attack map shows all the attempts malicious actors attemping toa ccess the Linux Virtual Machine</b>
![Linux Syslog Auth Failures](https://i.imgur.com/y3UDAhi.png)<br>

<b>This attack map shows all the attempts malicious actors attempting to access the Windows virtual machine</b>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/qPmicPj.png)<br>

<b> This attack map shows all MsSQL Authentication Failures</b>
![Mssql Auth Failures](https://i.imgur.com/9LX87jh.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-07-1 08:56:09
Stop Time 2024-07-2 08:56:09

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 30475
| Syslog                   | 10005
| SecurityAlert            | 5
| SecurityIncident         | 272
| AzureNetworkAnalytics_CL | 1766

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-07-11 08:26
Stop Time	2024-07-12 08:26

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 23469
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini, but effective, honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was configured to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the unsecured environment before security controls were applied and after implementing security measures. After the implementation of more robust security controls, there was a 22% reduction in Windows Security Events, a 99% reduction in Linux Events, and a 100% reduction in security alerts, incidents, and malicious inbound network traffic.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
