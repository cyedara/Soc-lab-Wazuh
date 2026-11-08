# Detection Use Cases

Each use case lists the objective, how it is tested, and what proves it worked. Detailed results go in [`../tests/`](../tests/).

## UC-01: Suspicious PowerShell Activity
- **Objective:** detect and investigate suspicious PowerShell execution.
- **Test (benign):** `powershell.exe -NoProfile -Command "Get-Date"`
- **Evidence needed:** Sysmon Event ID 1 + matching Wazuh alert (rule ID, timestamp).

## UC-02: Repeated Failed Logins
- **Objective:** identify repeated authentication failures / possible brute force.
- **Test:** generate controlled failed logins on the lab endpoint.
- **Evidence needed:** Security Event ID 4625 + Wazuh alert. Event 4624 (success) alone does not prove this use case.

## UC-03: Unusual Process Execution
- **Objective:** identify suspicious parent/child process relationships.
- **Test:** launch `notepad.exe` from PowerShell and inspect the Sysmon event.
- **Result so far:** Sysmon Event ID 1 (image `notepad.exe`, parent `powershell.exe`) was seen in Wazuh archives.

## UC-04: File Integrity Monitoring
- **Objective:** detect changes in an approved monitored test directory.
- **Test:** create/modify a test file in the monitored path (never real system files).
- **Evidence needed:** Wazuh syscheck (FIM) alert.

## UC-05: Suspicious Network Activity
- **Objective:** identify unusual connections or authorised scanning.
- **Tests (from Kali, authorised target only):**
  ```bash
  nc -vz -w 5 <windows-tailscale-ip> 44444
  nmap -sT -Pn -p 135,139,445,3389 <windows-tailscale-ip>
  ```
- **Observed:** `nc` to blocked port 44444 timed out; nmap showed 135/139/445 open and 3389 filtered.
- **Evidence needed:** Sysmon Event ID 3, firewall log DROP entry, Wazuh alert.
