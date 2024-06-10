# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I constructed a miniature honeynet in Azure and integrated log data from various sources into a Log Analytics workspace. This workspace is then utilized by Microsoft Sentinel to construct attack maps, activate alerts, and generate incidents. I assessed several security metrics within the unsecured environment over a 24-hour period, implemented security measures to fortify the environment, reevaluated metrics for another 24-hour period, and documented the outcomes below. The metrics presented include:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the initial "BEFORE" metrics phase, all resources were set up with internet exposure. The Virtual Machines had open Network Security Groups and built-in firewalls, while other resources had public endpoints accessible from the internet, eliminating the necessity for Private Endpoints.

During the subsequent "AFTER" metrics phase, Network Security Groups were fortified by blocking ALL traffic except for my admin workstation. Furthermore, all other resources were shielded by their built-in firewalls, and Private Endpoints were implemented for additional security.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

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

In this project, a small-scale honeynet was built in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to initiate alerts and incidents based on the logs collected. Security metrics were initially measured in the unsecured environment before implementing security controls, and then again after their implementation. Notably, the number of security events and incidents significantly decreased after the security controls were applied, indicating their effectiveness.

It's important to mention that if the network resources were heavily utilized by regular users, there might have been more security events and alerts generated within the 24-hour period following the implementation of security controls
