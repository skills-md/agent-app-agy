---
name: "sec-validate"
description: "Validate whether an AppSec finding is a true positive, false positive, or inconclusive. Use when the user types /sec:validate, provides a finding, or asks to confirm exploitability by code review or live testing."
---

# AppSec Finding Validation

Validate a security finding with source tracing and optional live testing.

## Input

The user must provide a finding path or pasted finding text. Extract vulnerability class, claim, endpoint, method, code location, and existing evidence.

## Workflow

1. Identify the vulnerability class and precise claim.
2. Read the affected code and supporting files.
3. Trace request flow from route registration through middleware to handler.
4. Check for direct and indirect protections such as auth middleware, ownership checks, schema constraints, ORM scoping, sanitizers, and validation helpers.
5. Confirm whether the vulnerable code path is reachable.
6. If a live instance is available and testing is authorized, use the `sec-proxy` workflow to capture and replay relevant HTTP requests.
7. Classify as `True Positive`, `True Positive (Confirmed)`, `False Positive`, or `Inconclusive`.

Read `VULNERABILITY_PATTERNS.md` for class-specific validation guidance.

## Completion

Output determination, confidence, evidence summary, code analysis, live test results if performed, and recommendation.
