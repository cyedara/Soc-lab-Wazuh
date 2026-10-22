# Playbook: Suspicious PowerShell Activity

## Objective
Investigate suspicious PowerShell execution on an authorized endpoint.

## Triage
1. Record the alert ID, timestamp, endpoint, user, and rule ID.
2. Review the process command line, parent process, and related events.
3. Determine whether the activity matches approved administration or testing.
4. Check for related alerts and unusual child processes.

## Evidence
Preserve relevant logs and timestamps in the approved evidence location.
Restrict access to sensitive evidence.

## Escalation
Escalate suspected malicious activity to the authorized incident lead.

## Containment
Only an authorized responder may approve containment actions.

## Closure
Document findings, impact, actions, outcome, and lessons learned.
