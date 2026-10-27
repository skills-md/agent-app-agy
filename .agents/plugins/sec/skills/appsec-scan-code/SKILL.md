---
name: "sec-scan-code"
description: "Run an AppSec SAST workflow for source code. It plans targeted scans and analyzes issues such as SQL injection, XSS, SSRF, BOLA, BFLA, IDOR, auth flaws, and OWASP categories. Use when the user types /sec:scan-code or asks for code vulnerability scanning."
---

# AppSec Code Scan

Find source-code security issues through a planned SAST workflow.

## Inputs

`depth` may be `quick`, `balanced`, or `full`; default is `quick`.

## Output Location

```bash
repo_name=$(basename "$(pwd)") && remote_url=$(git remote get-url origin 2>/dev/null || pwd) && short_hash=$(printf '%s' "$remote_url" | git hash-object --stdin | cut -c1-8) && repo_id="${repo_name}-${short_hash}" && short_sha=$(git rev-parse --short HEAD 2>/dev/null || date +%Y%m%d) && sec_repo_dir="$HOME/.secapp/repos/${repo_id}" && scan_dir="${sec_repo_dir}/scans/${short_sha}/code" && cache_dir="${sec_repo_dir}/cache" && mkdir -p "$scan_dir/findings" && echo "scan_dir=$scan_dir cache_dir=$cache_dir"
```

## Workflow

1. Read `$cache_dir/repo.md`; if it is missing, run the `sec-repo-context` workflow first.
2. Read `criteria/index.yaml` and the relevant criteria files for detected project types.
3. If `$scan_dir/plan.md` is missing, create a scan plan by following `prompts/planner.md`.
4. Create or reset `$scan_dir/nominations.md` from the plan, then follow `prompts/nominator.md`.
5. Analyze nominated files using `prompts/analyzer.md`; write one finding markdown file per candidate under `$scan_dir/findings/`.
6. Verify each finding with `prompts/verifier.md`; mark rejected findings clearly.

You may use `scripts/loop.sh` where it fits the local Antigravity environment, but do not depend on Claude-specific task orchestration.

## Completion

Report all finding files, count verified vs rejected findings, and summarize the highest-risk issues first.
