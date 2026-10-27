---
name: "sec-scan-deps"
description: "Run AppSec dependency vulnerability scanning and exploitability analysis. Use when the user types /sec:scan-deps, asks for SCA, vulnerable dependency checks, CVE checks, or lockfile security analysis."
---

# AppSec Dependency Scan

Scan dependency manifests and lockfiles for known vulnerabilities, then assess exploitability in repository context.

## Output Location

```bash
repo_name=$(basename "$(pwd)") && remote_url=$(git remote get-url origin 2>/dev/null || pwd) && short_hash=$(printf '%s' "$remote_url" | git hash-object --stdin | cut -c1-8) && repo_id="${repo_name}-${short_hash}" && short_sha=$(git rev-parse --short HEAD 2>/dev/null || date +%Y%m%d) && sec_repo_dir="$HOME/.secapp/repos/${repo_id}" && scan_dir="${sec_repo_dir}/scans/${short_sha}/deps" && cache_dir="${sec_repo_dir}/cache" && mkdir -p "$scan_dir/findings" && echo "scan_dir=$scan_dir cache_dir=$cache_dir"
```

## Workflow

1. Ensure repository context exists; run `sec-repo-context` first if `$cache_dir/repo.md` is missing.
2. Discover lockfiles and dependency files such as `package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `go.sum`, `go.mod`, `Gemfile.lock`, `poetry.lock`, `Pipfile.lock`, `requirements.txt`, `Cargo.lock`, and Maven/Gradle files.
3. If available, use the bundled agent guidance in `agents/discover`, `agents/scan`, `agents/analyze`, and `agents/summarize`.
4. If an external scanner binary is unavailable, use local lockfile evidence plus public advisory metadata only when network access is explicitly available; otherwise report that scanner-backed CVE resolution could not run.
5. Write candidates to `$scan_dir/candidates.json`, findings to `$scan_dir/findings/`, and a scan summary to `$scan_dir/report.md`.

## Completion

Report candidate count, confirmed exploitable findings, filtered false positives, and remediation guidance.
