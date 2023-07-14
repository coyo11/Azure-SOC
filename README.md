# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I built a Honeynet and SOC in Azure and ingested log sources from various resources into a Log Analytics workspace, which was then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured certain security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)



The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls completely open, and all other resources are deployed with public endpoints visible to the Internet.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/dgJa4lO.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/IWwl6vg.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/lWBckUv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
2023-06-25T22:54:36.2203916Z
Stop Time 2023-06-26T22:54:36.2203916Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 4520
| Syslog                   | 4876
| SecurityAlert            | 1
| SecurityIncident         | 98
| AzureNetworkAnalytics_CL | 470

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-07-12T18:26:16.2865994Z
Stop Time	2023-07-14T18:26:16.2865994Z

| Metric                   | Count
| -------------------------- | -----
| SecurityEvent              | 897
| Syslog                     | 2
| SecurityAlert              | 0
| SecurityIncident           | 0
| AzureNetworkAnalytics_CL   | 0


| Results                    | Count
| ------------------------   | -----
| SecurityEvents (Windows VM)| -80.15%
| Syslog  (Linux VMs)        | -99.96%
| (Defender For Cloud)       | -100%
| (Sentinel Incidents)       | -100%
| Inbound Malicious Flows    | -100%

## Conclusion

In this project, a Honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is also worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
