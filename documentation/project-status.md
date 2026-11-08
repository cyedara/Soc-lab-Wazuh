# Project Status

Legend: ✅ confirmed with evidence · 🟡 configured, not yet validated end to end · ❓ needs verification

| Area | Status | Notes |
|---|---|---|
| Wazuh Manager / Indexer / Dashboard | ✅ | Installed 10 Oct 2026; services active; dashboard at https://localhost |
| Windows agent (ID 001) | ✅ | Showed Active on 22 Oct 2026 |
| Sysmon | ✅ | Sysmon64 running; Event ID 1 visible in Wazuh archives |
| Windows Firewall logging | 🟡 | Log path and collection block configured; re-verify profile settings |
| Tailscale + Kali SSH | ✅ | Kali reachable; nc and nmap run from Kali |
| Intern role `soc_intern_readonly` | 🟡 | Role mapped to intern01–04; per-intern data isolation not proven |
| intern01 dashboard search | ❓ | Earlier `indices:data/read/search` permission error; recheck |
| UC-01 PowerShell | 🟡 | Telemetry only; alert not confirmed |
| UC-02 Failed logins | ❓ | 4625 events not yet shown |
| UC-03 Unusual process | ✅ | Sysmon ID 1 captured |
| UC-04 FIM | 🟡 | Monitored path to confirm |
| UC-05 Network | 🟡 | Traffic generated; alert correlation pending |
| Custom rules 100010–100013 | ❓ | Parent IDs unverified; run `wazuh-analysisd -t` |

## Next steps
1. Generate real 4625 events and confirm a Wazuh alert.
2. Validate custom-rule parent IDs against installed rules.
3. Test each intern login and check data isolation.
4. Save a test record per use case in `tests/`.

## Security notes
- Kali SSH currently accepts password logins; prefer key-only authentication.
- nmap showed SMB/RPC ports (135/139/445) reachable over Tailscale on the Windows endpoint; consider restricting them.
