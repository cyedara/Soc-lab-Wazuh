# Getting Started: Build the Lab Step by Step

Follow the steps in order. After each step there is a **Check** so you know it worked.

## What you need

- A Windows 10/11 laptop (admin rights, 16 GB RAM recommended)
- WSL with Ubuntu 24.04
- Sysmon installed on Windows
- A second machine running Kali Linux (optional, for UC-05)
- A free Tailscale account (optional, for remote testing)

## Step 1: Install Wazuh in Ubuntu (WSL)

Open PowerShell and start Ubuntu:

```powershell
wsl -l -v
wsl -d Ubuntu-24.04
```

Inside Ubuntu:

```bash
curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh
sudo bash ./wazuh-install.sh -a
```

Save the admin password shown at the end. Never put it on GitHub.

**Check:**
```bash
sudo systemctl is-active wazuh-indexer wazuh-manager wazuh-dashboard
```
All three should say `active`. Open https://localhost in a browser and log in.

## Step 2: Find the manager's IP

```bash
hostname -I
```
Note the first address (for example `172.17.x.x`). The Windows agent needs it.

## Step 3: Install the Wazuh agent on Windows

In the dashboard click **Deploy new agent**, choose Windows, enter the IP from Step 2 and an agent name, then copy the command it gives you into an **Administrator PowerShell** window. Then:

```powershell
NET START Wazuh
```

**Check:** in Ubuntu run `sudo /var/ossec/bin/agent_control -l`. Your agent should show **Active**. If it says "never connected", test the connection from Windows:

```powershell
Test-NetConnection <manager-ip> -Port 1514
Test-NetConnection <manager-ip> -Port 1515
```
Both should show `TcpTestSucceeded : True`.

## Step 4: Tell the agent which logs to send

Open the agent config as Administrator:

```powershell
notepad "C:\Program Files (x86)\ossec-agent\ossec.conf"
```

Add these blocks before the closing `</ossec_config>` line:

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

Save, then restart:

```powershell
Restart-Service -Name WazuhSvc
```

**Check:** `Get-Service Sysmon*` shows `Running`.

## Step 5: Turn on firewall logging

```powershell
Set-NetFirewallProfile -Profile Domain,Private,Public -LogBlocked True -LogFileName "%SystemRoot%\System32\LogFiles\Firewall\pfirewall.log"
Get-NetFirewallProfile | Select-Object Name, LogFileName, LogBlocked, LogAllowed
```

## Step 6: See your first event

On Windows:

```powershell
Start-Process notepad.exe
```

In Ubuntu:

```bash
sudo grep -i 'notepad' /var/ossec/logs/archives/archives.json | tail -n 3
```

If archives are empty, enable `<logall_json>yes</logall_json>` in `/var/ossec/etc/ossec.conf` on the manager and restart it with `sudo systemctl restart wazuh-manager`.

**Check:** you see a Sysmon "Process Create" entry for `notepad.exe`. This is UC-03.

## Step 7: Run the other use cases

Follow [`use-cases.md`](use-cases.md). For each one:
1. Run the test.
2. Find the source event on Windows.
3. Find it in Wazuh archives and alerts.
4. Fill in a record from [`../tests/test-record-template.md`](../tests/test-record-template.md).
5. Save screenshots in [`../evidence/`](../evidence/).

Remember: an event in `archives.json` is not an alert. A detection is proven only when the matching entry appears in `alerts.json` or the dashboard.

## Step 8 (optional): Remote testing from Kali

Install Tailscale on both machines and share the Kali node with your account, then:

```powershell
tailscale status
tailscale ping <kali-ip>
ssh kali@<kali-ip>
```

From Kali, only against your own endpoint:

```bash
nc -vz -w 5 <windows-tailscale-ip> 44444
nmap -sT -Pn -p 135,139,445,3389 <windows-tailscale-ip>
```

## Step 9: Give interns safe access

In the dashboard open **Security, Roles** and create a read-only role (for example `soc_intern_readonly`). Map each intern user to it. Never give interns `all_access`. Log in as each intern and confirm they can view alerts but cannot change settings.

## Common problems

| Problem | Fix |
|---|---|
| Services not active after reboot | Start them again with `sudo systemctl start wazuh-indexer wazuh-manager wazuh-dashboard` |
| Agent "never connected" | Check the manager IP (it can change after a WSL restart) and ports 1514/1515 |
| No Sysmon events in Wazuh | Check the `localfile` block and restart `WazuhSvc` |
| Event in archives but no alert | No rule matches it yet; write and test a custom rule |
| `git push` asks for a password | Use a GitHub personal access token |
