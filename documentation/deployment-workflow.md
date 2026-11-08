# Deployment Workflow

## 1. Install Wazuh (Ubuntu WSL)

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

## 2. Start and check services

```bash
sudo systemctl start wazuh-indexer wazuh-manager wazuh-dashboard
sudo systemctl is-active wazuh-indexer wazuh-manager wazuh-dashboard
sudo ss -lntp | grep -E ':1514|:1515|:443'
```

## 3. Install the Windows agent (elevated PowerShell)

Use the install command from the Wazuh dashboard ("Deploy new agent"), setting `WAZUH_MANAGER` to the WSL IP and `WAZUH_AGENT_NAME` to your agent name. Then:

```powershell
NET START Wazuh
Get-Service Sysmon*
```

## 4. Add log sources to the agent

Edit `C:\Program Files (x86)\ossec-agent\ossec.conf`:

```xml
<localfile>
  <location>Microsoft-Windows-Sysmon/Operational</location>
  <log_format>eventchannel</log_format>
</localfile>

<localfile>
  <location>C:\Windows\System32\LogFiles\Firewall\pfirewall.log</location>
  <log_format>syslog</log_format>
</localfile>
```

Restart the agent:

```powershell
Restart-Service -Name WazuhSvc
```

## 5. Confirm the agent is connected

```bash
sudo /var/ossec/bin/agent_control -l
sudo /var/ossec/bin/agent_control -i 001
```

## 6. Remote test access

```powershell
tailscale status
ssh kali@<kali-tailscale-ip>
```

## Troubleshooting note

Early on, the dashboard showed agent 001 as **never connected**. Connectivity from Windows to the manager was checked with `Test-NetConnection <wsl-ip> -Port 1514/1515/443` (all succeeded). The agent later showed **Active**.
