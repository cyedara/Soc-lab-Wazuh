# Architecture

## Components

- **Windows endpoint** (`DESKTOP-6DE77JA`, Windows 11 Pro Education)
  - Sysmon64 service, channel `Microsoft-Windows-Sysmon/Operational`
  - Wazuh agent 4.14.8, name `silasandvalli_SOC_LAB`, agent ID 001
  - Windows Firewall log: `C:\Windows\System32\LogFiles\Firewall\pfirewall.log`
- **Ubuntu 24.04 (WSL)**: Wazuh Manager, Indexer, Dashboard (https://localhost)
  - Agent ports: 1514 (events), 1515 (enrolment); dashboard: 443
- **Kali Linux** over Tailscale + SSH, used to generate authorised test traffic

## Data flow

```
Sysmon / Security log / pfirewall.log
        -> Wazuh agent (Windows)
        -> Wazuh Manager (WSL, ports 1514/1515)
        -> archives.json / alerts.json
        -> Wazuh Indexer -> Dashboard
```

```
Kali --(Tailscale)--> Windows endpoint  (nc / nmap test traffic)
```

## Useful log locations (Ubuntu)

- `/var/ossec/logs/archives/archives.json`: every event received
- `/var/ossec/logs/alerts/alerts.json`: only events that matched an alert rule
- `/var/ossec/etc/rules/local_rules.xml`: custom rules

> An archived event is not automatically an alert.
