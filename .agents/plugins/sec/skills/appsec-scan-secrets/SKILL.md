---
name: "sec-scan-secrets"
description: "Run AppSec secrets and credential scanning. Use when the user types /sec:scan-secrets or asks to find exposed tokens, passwords, API keys, .env values, private keys, or hardcoded credentials."
---

# AppSec Secrets Scan

Scan the repository for exposed credentials and assess each candidate in context.

## Output Location

```bash
repo_name=$(basename "$(pwd)") && remote_url=$(git remote get-url origin 2>/dev/null || pwd) && short_hash=$(printf '%s' "$remote_url" | git hash-object --stdin | cut -c1-8) && repo_id="${repo_name}-${short_hash}" && short_sha=$(git rev-parse --short HEAD 2>/dev/null || date +%Y%m%d) && sec_repo_dir="$HOME/.secapp/repos/${repo_id}" && scan_dir="${sec_repo_dir}/scans/${short_sha}/secrets" && cache_dir="${sec_repo_dir}/cache" && mkdir -p "$scan_dir/findings" && echo "scan_dir=$scan_dir cache_dir=$cache_dir"
```

## Workflow

1. Ensure repository context exists; run `sec-repo-context` first if `$cache_dir/repo.md` is missing.
2. Scan source, config, scripts, examples, CI files, and documentation for high-entropy tokens, private keys, credentials, webhook secrets, cloud keys, JWT secrets, and `.env` values.
3. Use `agents/scan`, `agents/analyze`, and `agents/summarize` as detailed guidance.
4. Avoid printing full secret values. Redact all but a minimal prefix/suffix needed for evidence.
5. Write candidates to `$scan_dir/candidates.json`, findings to `$scan_dir/findings/`, and a scan summary to `$scan_dir/report.md`.

## Completion

Report confirmed or likely secrets, location, exposure risk, rotation guidance, and false-positive filtering.
