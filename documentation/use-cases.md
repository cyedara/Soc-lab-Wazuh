# Detection Use Cases

## UC-01: Suspicious PowerShell Activity
Objective: Detect and investigate suspicious PowerShell command execution.
Validation: Generate an authorized lab event and confirm the expected alert.

## UC-02: Repeated Failed Logins
Objective: Identify repeated authentication failures and investigate possible brute-force activity.
Validation: Generate controlled failed-login events on a lab endpoint.

## UC-03: Unusual Process Execution
Objective: Identify suspicious process relationships and unexpected execution paths.
Validation: Generate a benign, authorized process event and inspect the resulting telemetry.

## UC-04: File-Integrity Changes
Objective: Detect changes to files in an approved monitored test directory.
Validation: Create or modify a test file and confirm the FIM alert.

## UC-05: Suspicious Network Activity
Objective: Identify unusual connections or authorized lab scanning indicators.
Validation: Generate approved test traffic and verify that the available telemetry supports detection.
