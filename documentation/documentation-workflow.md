# Keeping the project documentation current

[Portfolio](../README.md) · [Current deployment](current-state.md) · [Change history](../CHANGELOG.md)

The portfolio uses three connected records: the current deployment, a dated history of changes, and evidence/incident details. A future change should update all affected records rather than leaving the README as the only source of state.

## Record a completed change

| Field | Information to retain |
|---|---|
| Date and component | When the work was completed; host/container/application affected |
| Before | Previous setting, version, symptom, or limitation |
| Change and reason | Actual action taken and why it addressed the problem |
| Verification | Command/result and user-visible check; distinguish configured controls from tests performed |
| Recovery | Usable backup or reversal path, if relevant |
| Remaining work | Any incomplete validation or hardening item |
| Evidence | Sanitized screenshot or short output capture with its date |

For an incident, also record the observed impact, investigation, supported cause, fix, and result. If the cause was not established, preserve that uncertainty alongside the confirmed recovery.

## Update the portfolio

1. Add the dated outcome to [CHANGELOG](../CHANGELOG.md).
2. Update the relevant row/path/version in [current deployment](current-state.md).
3. Revise the affected configuration page, incident report, or runbook.
4. Add sanitized evidence and link it from the report/index.
5. Check relative links, code fences, image rendering, and the changed files for secrets before publishing.

An application release tag can change without changing its name. Record the exact version or image digest when available. Keep full `.env` files, API credentials, private SSH material, database dumps, and personal media outside GitHub.

This process records completed work. Planned improvements stay on the roadmap until implementation and verification are documented.
