# Custom Wazuh Rules

Reviewed copy of `/var/ossec/etc/rules/local_rules.xml`. The live file stays on the Wazuh manager.

## Existing rule (preserve, do not overwrite)

```xml
<group name="local,syslog,sshd,">
  <rule id="100001" level="5">
    <if_sid>5716</if_sid>
    <srcip>1.1.1.1</srcip>
    <description>sshd: authentication failed from IP 1.1.1.1.</description>
    <group>authentication_failed,pci_dss_10.2.4,pci_dss_10.2.5,</group>
  </rule>
</group>
```

## Draft rules 100010–100013

Planned for PowerShell, repeated failed logins, firewall drops and network scanning. **Not confirmed working**: parent rule IDs must be checked against the installed ruleset and real event JSON first.

## Validate before deploying

```bash
sudo cp /var/ossec/etc/rules/local_rules.xml /var/ossec/etc/rules/local_rules.xml.backup
sudo /var/ossec/bin/wazuh-analysisd -t
sudo systemctl restart wazuh-manager
```

Only restart if the syntax test passes.
