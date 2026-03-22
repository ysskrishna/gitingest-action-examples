# gitingest-action Examples

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-gitingest--action-blue?logo=github)](https://github.com/marketplace/actions/gitingest)
[![GitHub Action](https://img.shields.io/badge/action-gitingest-orange?logo=github-actions)](https://github.com/ysskrishna/gitingest-action)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Ready-to-use GitHub Actions workflows demonstrating all features of [gitingest-action](https://github.com/marketplace/actions/gitingest) — a tool that creates comprehensive digests of your codebase for AI analysis, documentation, and code review.

## Quick Start

Add this to any workflow to analyze your repository:

```yaml
- uses: actions/checkout@v4
- uses: ysskrishna/gitingest-action@v1
```

Browse the [examples](#-usage-examples) below for advanced patterns and features.

## Inputs

| Input | Type | Default | Description |
|-------|------|---------|-------------|
| `source` | string | `.` | Repository URL or local path to analyze |
| `output-dir` | string | `gitingest-output` | Directory where output files will be saved |
| `include-patterns` | string | _(none)_ | File patterns to include (glob syntax, one per line) |
| `exclude-patterns` | string | _(none)_ | File patterns to exclude (glob syntax, one per line) |
| `max-file-size` | string | `10485760` | Maximum file size in bytes (default: 10MB) |
| `branch` | string | _(default branch)_ | Branch to analyze (remote repos only) |
| `tag` | string | _(none)_ | Tag/release to analyze (remote repos only) |
| `token` | string | _(none)_ | GitHub token for private repositories |
| `enable-job-summary` | boolean | `true` | Display digest summary in workflow run |
| `enable-annotations` | boolean | `true` | Add workflow annotations for key metrics |

## Outputs

| Output | Description |
|--------|-------------|
| `summary-file` | Path to summary.txt (repository overview and statistics) |
| `tree-file` | Path to tree.txt (directory structure visualization) |
| `content-file` | Path to content.txt (full repository content) |

## Output Files

The action generates three files in the output directory:

- **`summary.txt`** — High-level repository statistics including file counts, language distribution, and size metrics
- **`tree.txt`** — Visual directory tree showing repository structure
- **`content.txt`** — Full concatenated content of all analyzed files

## Usage Examples

### Master Workflow

| Workflow | Description | Key Features |
|----------|-------------|--------------|
| [**🚀 Run All Examples**](.github/workflows/run-all-examples.yml) | **Trigger all 8 workflows at once** | Batch execution with single click |

### Individual Examples

| Workflow | Description | Key Features |
|----------|-------------|--------------|
| [**Analyze Remote Repository**](.github/workflows/analyze-remote-repo.yml) | Analyze a repo you don't own | `source`, `branch` |
| [**Filter Files with Patterns**](.github/workflows/filter-files-with-patterns.yml) | Scope analysis to specific file types | `include-patterns`, `exclude-patterns` |
| [**Analyze Specific Tag**](.github/workflows/analyze-specific-tag.yml) | Pin analysis to a tagged release | `tag` |
| [**Analyze Private Repository**](.github/workflows/analyze-private-repo.yml) | Use token for private repo access | `token` |
| [**Save Digest as Artifact**](.github/workflows/save-digest-as-artifact.yml) | Persist full output as downloadable artifact | `output-dir`, `upload-artifact` |
| [**Selective Artifact Upload**](.github/workflows/selective-artifact-upload.yml) | Upload only summary/tree to save space | Artifact path filtering |
| [**Monorepo - Scoped Analysis**](.github/workflows/monorepo-scoped-analysis.yml) | Analyze subdirectory with file filtering | Local path, patterns, `max-file-size` |
| [**Scheduled Weekly Digest**](.github/workflows/scheduled-weekly-digest.yml) | Run on cron schedule with artifacts | `schedule` trigger, dated artifacts |

### Running the Examples

**Option 1: Run all examples at once** (recommended for testing)
1. Fork this repository
2. Navigate to **Actions** tab in your fork
3. Select **"Run All Examples"** workflow
4. Click **Run workflow** button
5. All 8 example workflows will be triggered simultaneously

**Option 2: Run individual examples**
1. Navigate to **Actions** tab
2. Select any specific workflow from the list
3. Click **Run workflow** button
4. View the workflow run to see the digest output

Each workflow uses `workflow_dispatch` so you can trigger it manually and see results immediately.

## Large Repositories

The action has built-in limits to prevent excessive processing. Understanding these helps optimize analysis for large codebases:

| Metric | Default Limit | Description |
|--------|---------------|-------------|
| **Max File Size** | 10 MB | Files larger than this are skipped |
| **Max Repo Size** | 500 MB | Total size limit for analyzed files |
| **Max Files** | 10,000 | Maximum number of files to process |

### Recommendations for Large Repositories

1. **Use pattern filters** — Analyze only relevant file types:
   ```yaml
   include-patterns: |
     *.ts
     *.tsx
     *.js
   exclude-patterns: |
     node_modules/*
     dist/*
     *.test.ts
   ```

2. **Analyze subdirectories** — Focus on specific packages in monorepos:
   ```yaml
   source: './packages/core'
   ```

3. **Increase file size limit** — Process larger files if needed:
   ```yaml
   max-file-size: '20971520'  # 20MB
   ```

4. **Skip large binaries** — Always exclude build artifacts, dependencies, and media files:
   ```yaml
   exclude-patterns: |
     *.zip
     *.tar.gz
     *.png
     *.jpg
     node_modules/*
     vendor/*
     dist/*
     build/*
   ```

## Real-World Use Cases

- **AI Code Review** — Feed the digest to Claude or GPT for comprehensive code analysis
- **Documentation** — Generate up-to-date repository overviews automatically
- **Onboarding** — Provide new developers with complete codebase snapshots
- **Release Notes** — Create digests for specific tags to document releases
- **Security Audits** — Analyze full repository content for security reviews
- **Migration Planning** — Understand codebase structure before major refactors

## Related Resources

- [gitingest-action on GitHub Marketplace](https://github.com/marketplace/actions/gitingest)
- [gitingest-action Repository](https://github.com/ysskrishna/gitingest-action)
- [gitingest CLI Tool](https://github.com/coderamp-labs/gitingest)

## License

MIT — See [LICENSE](LICENSE) file for details.

---

**Note:** This is an examples repository. For action source code and issues, visit [ysskrishna/gitingest-action](https://github.com/ysskrishna/gitingest-action).
