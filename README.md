# Secure Remote SOC Lab

A beginner-friendly, personal Security Operations Center (SOC) lab built with **Wazuh**. It shows how to collect logs from a Windows laptop, watch them in a dashboard, run safe test attacks from a Kali machine, and write up what you find.

> **Scope:** personal lab only. No company-server setup is included.

## Who is this for?

Students and new SOC analysts who want hands-on practice with detection and incident response, using free tools and one laptop.

## Start here

1. Read [`documentation/getting-started.md`](documentation/getting-started.md) for the step-by-step build guide.
2. Read [`documentation/use-cases.md`](documentation/use-cases.md) for the five detection exercises.
3. Follow [`playbooks/incident-response-template.md`](playbooks/incident-response-template.md) when you investigate an alert.
4. Record each exercise using [`tests/test-record-template.md`](tests/test-record-template.md).
5. Check [`documentation/project-status.md`](documentation/project-status.md) to see what is proven and what is still open.

## What you will build

| Component | Role |
|---|---|
| Windows 11 laptop | Monitored endpoint (Wazuh agent + Sysmon + Firewall logging) |
| Ubuntu 24.04 on WSL | Wazuh 4.14.8: Manager, Indexer, Dashboard |
| Kali Linux | Authorised test traffic source (over Tailscale + SSH) |
| Tailscale | Private network between the laptop and Kali |
| GitHub | Documentation and version control |

## Repository map

| Folder | Contents |
|---|---|
| [`documentation/`](documentation/) | Getting started, architecture, use cases, status, workflow |
| [`rules/`](rules/) | Custom Wazuh rules |
| [`playbooks/`](playbooks/) | Incident-response playbooks |
| [`tests/`](tests/) | Test records per use case |
| [`evidence/`](evidence/) | Screenshots proving each step |

## Detection use cases

| ID | Use case | Status |
|---|---|---|
| UC-01 | Suspicious PowerShell activity | Telemetry captured; alert not yet confirmed |
| UC-02 | Repeated failed logins | Needs 4625 events generated and matched |
| UC-03 | Unusual process execution | Sysmon Event ID 1 captured in Wazuh |
| UC-04 | File integrity monitoring | Needs monitored path confirmed |
| UC-05 | Suspicious network activity | Test traffic generated; alert correlation pending |

## Safety rules

- Only test systems you own or are authorised to test.
- Never commit passwords, private keys, Tailscale auth keys or raw sensitive logs.
