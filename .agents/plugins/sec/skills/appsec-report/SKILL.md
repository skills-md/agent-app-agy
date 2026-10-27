---
name: "sec-report"
description: "Create a combined AppSec report from repo context, dependency scan, secrets scan, and code scan results. Use when the user types /sec:report or asks for a consolidated vulnerability report."
---

# AppSec Combined Report

Aggregate AppSec scan results into one prioritized report.

## Output Location

```bash
repo_name=$(basename "$(pwd)") && remote_url=$(git remote get-url origin 2>/dev/null || pwd) && short_hash=$(printf '%s' "$remote_url" | git hash-object --stdin | cut -c1-8) && repo_id="${repo_name}-${short_hash}" && short_sha=$(git rev-parse --short HEAD 2>/dev/null || date +%Y%m%d) && sec_repo_dir="$HOME/.secapp/repos/${repo_id}" && scans_dir="${sec_repo_dir}/scans/${short_sha}" && cache_dir="${sec_repo_dir}/cache" && echo "scans_dir=$scans_dir cache_dir=$cache_dir"
```

Write the combined report to `$scans_dir/report.md`.

## Workflow

1. Read `$cache_dir/repo.md` if present.
2. Discover scan directories under `$scans_dir`: `deps/`, `secrets/`, and `code/`.
3. Read all finding files from each `findings/` directory.
4. Exclude rejected, clean, and false-positive findings.
5. Prioritize by severity, exploitability, sensitive data exposure, and business criticality.
6. Use `report-template.md` for structure.

## Completion

Return the combined report path, high-risk count, medium-risk count, scans included, and the top remediation priorities.
