# PowerShell Scripts

Small administrative examples. Review and adapt them in a controlled environment before use.

## AddTo-Nagios

Creates a Nagios XI host and requests configuration application (`applyconfig=1`). Requires PowerShell, DNS access, a trusted HTTPS certificate, and an authorized Nagios XI API account with only the permissions needed for the operation.

Set `NAGIOS_XI_BASE_URL` to the server origin (for example, `https://nagios.example.com`) and supply `NAGIOS_XI_API_KEY` to the current process from an approved secret store. Do not paste real credentials into scripts, shell history, README examples, or command-line arguments. Clear the environment variable when finished. Environment variables are plaintext process configuration, not a secret vault.

The existing extensionless script can be invoked as a script block:

```powershell
& ([scriptblock]::Create((Get-Content -LiteralPath ./AddTo-Nagios -Raw))) -WhatIf
```

Omit `-WhatIf` to send the request after confirmation. WhatIf still prompts for a hostname and performs DNS resolution. HTTPS origins only are accepted; redirects are refused. The script retains the existing check and notification settings; review those settings before use. A completed HTTP request does not establish that Nagios accepted or applied the configuration. Verify in Nagios XI before retrying, particularly after a timeout.

Nagios authentication currently uses an API key in the query string. HTTPS protects transport, but server/proxy access logs, diagnostics, transcripts, and PowerShell error records may retain the URL. Do not enable request tracing or share raw errors. The script suppresses raw responses and replaces displayed request failures with a generic message; this does not guarantee erasure of credentials or diagnostic records from memory. Use restricted log access and appropriate URL redaction at the server/proxy.

## Historical credential exposure

Review of fetched repository history on 2026-10-27 found a literal Nagios API credential in the two 2020 commits that added and updated `AddTo-Nagios`. The 2026-10-13 change removed the literal from the default branch's current script. The `patch-1` branch still points to the older version. Removing a literal from current source does not remove it from history or other copies.

Credential validity, revocation, rotation, and past use have not been verified. The credential owner should revoke/rotate any affected credential through the Nagios administration process, review available access logs, and check dependent integrations. Do not test the historical value against a live service. No history rewrite or branch deletion is part of this hardening; those require a separate decision and do not replace revocation.

## Other examples

- `Connect-365.ps1` is a historical Office 365 connection example using MSOnline and Basic-authentication Exchange remoting. It has not been modernized or runtime-validated in this review.
- `Get-Adgroups-ByOwner.ps1` is an incomplete Active Directory example with a malformed SearchBase assignment. It requires repair and environment-specific configuration before use.

Validation for this hardening uses PowerShell parsing and mocked network calls only. No Nagios endpoint or real API credential is used.
