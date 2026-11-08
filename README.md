# Secure Remote SOC Lab

A personal Security Operations Center (SOC) lab built on Wazuh, used to practise detection, investigation and incident response in a safe, authorised environment.

> **Scope:** this repository covers the personal lab only. No company-server configuration is included.

## Lab stack

| Component | Role |
|---|---|
| Windows 11 Pro Education laptop | Monitored endpoint (Wazuh agent + Sysmon + Windows Firewall logging) |
| Ubuntu 24.04 on WSL | Wazuh 4.14.8 all-in-one: Manager, Indexer, Dashboard |
| Kali Linux (partner's machine) | Authorised test traffic source, reached over Tailscale + SSH |
| Tailscale | Private network between the laptop and Kali |
| GitHub | Documentation and version control |

## Repository map

| Folder | What goes there |
|---|---|
| [`documentation/`](documentation/) | Architecture, use cases, project status, deployment workflow |
| [`rules/`](rules/) | Reviewed copy of custom Wazuh rules |
| [`playbooks/`](playbooks/) | Incident-response playbooks and templates |
| [`tests/`](tests/) | Test records for each use case |
| [`evidence/`](evidence/) | Screenshots and exported events proving each step |

## Detection use cases

| ID | Use case | Status |
|---|---|---|
| UC-01 | Suspicious PowerShell activity | Telemetry captured; alert not yet confirmed |
| UC-02 | Repeated failed logins | Needs 4625 events generated and matched |
| UC-03 | Unusual process execution | Sysmon Event ID 1 captured in Wazuh |
| UC-04 | File integrity monitoring | Needs monitored path confirmed |
| UC-05 | Suspicious network activity | Test traffic generated; alert correlation pending |

See [`documentation/project-status.md`](documentation/project-status.md) for the full, honest status.

## Safety

Only test against systems you own or are explicitly authorised to test. Never commit private keys, passwords, Tailscale auth keys or raw sensitive logs.
