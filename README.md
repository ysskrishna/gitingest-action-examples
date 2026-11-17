# gitingest-action Examples

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-gitingest--action-blue?logo=github)](https://github.com/marketplace/actions/gitingest)
[![GitHub Action](https://img.shields.io/badge/action-gitingest-orange?logo=github-actions)](https://github.com/ysskrishna/gitingest-action)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Author site](https://img.shields.io/badge/author-ysskrishna.space-informational)](https://ysskrishna.space)

Ready-to-use GitHub Actions workflows demonstrating common [gitingest-action](https://github.com/marketplace/actions/gitingest) patterns — a tool that creates comprehensive digests of your codebase for AI analysis, documentation, and code review.

## Quick Start

Analyze any repository remotely without cloning:

```yaml
- uses: ysskrishna/gitingest-action@v1
  with:
    source: 'https://github.com/owner/repo'
```

Browse the [examples](#usage-examples) below for advanced patterns and features.

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
| [**🚀 Run All Examples**](.github/workflows/run-all-examples.yml) | **Trigger all 7 workflows at once** | Batch execution with single click |

### Individual Examples

| Workflow | Description | Key Features |
|----------|-------------|--------------|
| [**Analyze Remote Repository**](.github/workflows/analyze-remote-repo.yml) | Analyze a repo you don't own | `source`, `branch` |
| [**Filter Files with Patterns**](.github/workflows/filter-files-with-patterns.yml) | Scope analysis to specific file types | `include-patterns`, `exclude-patterns` |
| [**Analyze Specific Tag**](.github/workflows/analyze-specific-tag.yml) | Pin analysis to a tagged release | `tag` |
| [**Save Digest as Artifact**](.github/workflows/save-digest-as-artifact.yml) | Persist full output as downloadable artifact | `output-dir`, `upload-artifact` |
| [**Selective Artifact Upload**](.github/workflows/selective-artifact-upload.yml) | Upload only summary/tree to save space | Artifact path filtering |
| [**Monorepo - Analyze Backend Only**](.github/workflows/monorepo-scoped-analysis.yml) | Analyze specific directory of a monorepo with filtering | `source` (subdirectory), `include-patterns`, `exclude-patterns`, `max-file-size` |
| [**Scheduled Weekly Digest**](.github/workflows/scheduled-weekly-digest.yml) | Run on cron schedule with artifacts | `schedule` trigger, dated artifacts |

### Running the Examples

**Option 1: Run all examples at once** (recommended for testing)
1. Fork this repository
2. Navigate to **Actions** tab in your fork
3. Select **"Run All Examples"** workflow
4. Click **Run workflow** button
5. All 7 example workflows will be triggered simultaneously

**Option 2: Run individual examples**
1. Navigate to **Actions** tab
2. Select any specific workflow from the list
3. Click **Run workflow** button
4. View the workflow run to see the digest output

Each workflow uses `workflow_dispatch` so you can trigger it manually and see results immediately.

## Private Repository Access

This examples repository does not include a dedicated private-repo workflow by default, because private repository analysis requires user-managed credentials.

If you need to analyze a private repository, create your own workflow and pass a token to `gitingest-action`:

```yaml
- uses: ysskrishna/gitingest-action@v1
  with:
    source: 'https://github.com/owner/private-repo'
    token: ${{ secrets.PRIVATE_REPO_TOKEN }}
```

Recommended setup:

- Store the token in your repository or organization Actions secrets as `PRIVATE_REPO_TOKEN`
- Grant least-privilege read access to the target private repository
- Avoid hardcoding tokens directly in workflow files

## Security Hardening

For production workflows, prefer pinning third-party actions to commit SHAs instead of floating tags (for example, `@v1` or `@v4`) to reduce supply-chain risk.

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
   # For remote repos, use the full tree URL
   source: 'https://github.com/owner/repo/tree/main/backend'

   # For local repos after checkout
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

## Credits

Examples for [gitingest-action](https://github.com/ysskrishna/gitingest-action). Built on [gitingest](https://github.com/coderamp-labs/gitingest).

## Built and maintained by

gitingest-action Examples is built and maintained by:

| | Name | GitHub | Role |
|---|------|--------|------|
| <img src="https://github.com/ysskrishna.png?size=120" width="60" height="60" alt="Y. Siva Sai Krishna"> | **Y. Siva Sai Krishna** | [@ysskrishna](https://github.com/ysskrishna) | Creator |

## Support

If you find this project helpful:

- ⭐ Star the repository
- 🐛 [Report issues](https://github.com/ysskrishna/gitingest-action-examples/issues)
- 🔀 Submit pull requests
- 💝 [Sponsor on GitHub](https://github.com/sponsors/ysskrishna)

## License

MIT © [Y. Siva Sai Krishna](https://github.com/ysskrishna) — see [LICENSE](LICENSE) for details.

---

<p align="left">
  <a href="https://github.com/ysskrishna">Author's GitHub</a> •
  <a href="https://linkedin.com/in/ysskrishna">Author's LinkedIn</a> •
  <a href="https://ysskrishna.space">Author's site</a> •
  <a href="https://github.com/marketplace/actions/gitingest">GitHub Marketplace</a> •
  <a href="https://github.com/ysskrishna/gitingest-action">gitingest-action</a> •
  <a href="https://github.com/coderamp-labs/gitingest">gitingest Documentation</a> •
  <a href="https://github.com/ysskrishna/gitingest-action-examples/issues">Report Issues</a>
</p>
