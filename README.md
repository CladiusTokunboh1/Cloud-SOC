# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/CladiusTokunboh1/Cloud-SOC/assets/161155502/cc930a52-8c21-46f7-92c3-97e059ab6c01)

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

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

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![](https://github.com/CladiusTokunboh1/Cloud-SOC/assets/161155502/14d22345-b4b3-421e-a0f6-072e0bea82d7)
![before syslog-linux-ssh-auth-fail 2-12-24](https://github.com/CladiusTokunboh1/Cloud-SOC/assets/161155502/0dd55001-6efe-4ff6-a5b4-c40fc8b2957d)
![](https://github.com/CladiusTokunboh1/Cloud-SOC/assets/161155502/a6b88700-7697-4eee-8147-17757899dcdc)

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-02-09 20:25
Stop Time  2024-02-10 20:25

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 36634
| Syslog                   | 703
| SecurityAlert            | 25
| SecurityIncident         | 149
| AzureNetworkAnalytics_CL | 3320

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-02-16 01:25
Stop Time	 2024-02-17 01:25

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10239
| Syslog                   | 5
| SecurityAlert            | 2
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

Wth this project, a miniature honeynet was set up within Microsoft Azure, with log sources seamlessly integrated into a Log Analytics workspace. Microsoft Sentinel played a key role in initiating alerts and incident creation based on the ingested logs. Furthermore, metrics were assessed in the insecure environment both prior to the application of security controls and subsequently. Significantly, the implementation of security measures resulted in a marked reduction in the occurrence of security events and incidents, highlighting their effectiveness. 

It's worth noting that if network resources were heavily utilized by regular users, there is a likelihood of generating more security events and alerts within the 24-hour period following the implementation of security controls.
