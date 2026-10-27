---
name: "appsec-proxy"
description: "Use an HTTP interception proxy for authorized AppSec live validation. Use when the user types /sec:appsec-proxy or when sec:appsec-validate needs captured request/response evidence from a live application."
---

# AppSec Validation Proxy

Use a local HTTP proxy only for systems where the user has authorization to test.

## Workflow

1. Confirm the target host and scope.
2. Start the configured proxy tooling if available in the environment.
3. Capture authenticated requests needed to validate a finding.
4. Replay or modify requests only within the approved scope.
5. Preserve concise request/response evidence with secrets and tokens redacted.

If no proxy tooling is installed, report the missing dependency and continue with source-code validation only.

## Completion

Return proxy status, target scope, captured evidence paths or summaries, and any limitations.
