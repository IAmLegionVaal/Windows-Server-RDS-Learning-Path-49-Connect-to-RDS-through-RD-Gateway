# Windows Server RDS Learning Path 49 — Connect to RDS through RD Gateway

**Level:** Advanced · **Module:** 49/70

## Goal
Connect from a simulated external client to the RDS collection through RD Gateway over HTTPS.

## Setup
1. Place `EXTCL01` on a network without direct reachability to RDS01.
2. Trust the Gateway certificate chain and resolve the Gateway FQDN.
3. In mstsc Advanced settings, configure the RD Gateway FQDN.
4. Disable bypass for local addresses for this test.
5. Connect to the RDS collection with an approved user.
6. Verify the Gateway path and Session Host.
7. Confirm direct TCP 3389 to RDS01 is unavailable from EXTCL01.

```powershell
Test-NetConnection <gateway-fqdn> -Port 443
Test-NetConnection rds01.corp.lab -Port 3389
Get-RDUserSession -ConnectionBroker 'RDS01.corp.lab'
Get-WinEvent -LogName 'Microsoft-Windows-TerminalServices-Gateway/Operational' -MaxEvents 50
```

## Evidence
Capture client Gateway settings, certificate trust, successful HTTPS/Gateway connection, Session Host correlation and direct-RDP denial under `evidence/`.

## Troubleshooting
If 443 works but connection fails, inspect certificate trust, CAP, RAP, DNS and Gateway events. Do not open direct RDP as a workaround.

## Security
External RDS access must traverse Gateway with trusted TLS and approved authorization policies.

## Rollback
Remove the external test route and client settings; keep the Gateway internal until the security design is accepted.

## Next
`Windows-Server-RDS-Learning-Path-50-Validate-RD-Gateway-Event-Logs`
