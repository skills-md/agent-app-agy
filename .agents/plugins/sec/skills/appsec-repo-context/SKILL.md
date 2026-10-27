---
name: "sec-repo-context"
description: "Build AppSec repository context for a codebase: detect projects, map architecture, identify sensitive data, document business criticality, and write a reusable repo.md context file. Use when the user types /sec:repo-context, asks for repository context before security analysis, or needs an AppSec-oriented codebase overview."
---

# AppSec Repository Context

Build shared repository context for the AppSec workflow. Work only from local repository files; do not use web search.

## Output Location

Compute paths from the current repository:

```bash
repo_name=$(basename "$(pwd)") && remote_url=$(git remote get-url origin 2>/dev/null || pwd) && short_hash=$(printf '%s' "$remote_url" | git hash-object --stdin | cut -c1-8) && repo_id="${repo_name}-${short_hash}" && cache_dir="$HOME/.secapp/repos/${repo_id}/cache" && mkdir -p "$cache_dir" && echo "cache_dir=$cache_dir"
```

The final file is `$cache_dir/repo.md`.

## Workflow

1. If `$cache_dir/repo.md` already exists, read it and report its path.
2. Read `detector.md` from this skill directory and follow it against the current repository.
3. Read `summarizer.md` and summarize every detected project.
4. Write `$cache_dir/repo.md` using `template-repo.md`.
5. Re-read the output and verify it includes project detection, architecture summary, sensitive data types, business criticality, component map, and evidence.

## Completion

Return the path to `repo.md`, list detected projects, and summarize the highest-value AppSec context gathered.
