# 0002 - Exchange product data as a file instead of an API

Example ADR. Delete it when the template is used.

## Status
Accepted

## Date
2026-01-15

## Author
Jane Doe

## Context
The external partner imports product data only from a CSV file on its SFTP server. It fetches the file every hour. The partner offers no API for product imports.

## Decision
Generate one CSV file in the partner's template and upload it to the fixed path on the partner's SFTP server.

## Consequences
- Positive: No dependency on a partner API; the file can be checked locally before upload.
- Known cost or risk: Changes reach the partner with up to one hour delay; import errors are only visible in the partner's system.
- Follow-up: Review when the partner offers an import API.
