# Building a SOC + Honeynet in Azure (Live Traffic)
![image](https://github.com/Karisky-M/Cloud-SOC/assets/157313566/17d8e13d-8361-4a71-bb60-9225fed24bc8)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![image](https://github.com/Karisky-M/Cloud-SOC/assets/157313566/3d4ab1e9-4d4e-4192-97b9-81af1ae4cf21)



## Architecture After Hardening / Security Controls
![image](https://github.com/Karisky-M/Cloud-SOC/assets/157313566/d5ad1e00-ea69-4eca-9081-6f8b57704a98)

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
![Before-nsg-malicious-allowed-in](https://github.com/Karisky-M/Cloud-SOC/assets/157313566/3a773820-2194-4567-9c4f-f8a0c9594e7d)
![Before-linux-ssh-auth-fail](https://github.com/Karisky-M/Cloud-SOC/assets/157313566/813f6c78-dc59-4f3b-9c9b-67b8d434d911)
![Before-windows-rdp-auth-fail](https://github.com/Karisky-M/Cloud-SOC/assets/157313566/906e2d5d-a27c-49f9-9fe2-fd43502093fd)



## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-03-21 14:02:05
Stop Time 2024-03-22 14:02:05

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 50058
| Syslog                   | 11290
| SecurityAlert            | 25
| SecurityIncident         | 260
| AzureNetworkAnalytics_CL | 2083

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-03-26 17:22:38
Stop Time	2024-03-27 17:22:38

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8390
| Syslog                   | 2874
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
