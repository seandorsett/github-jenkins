# GitHub Actions Importer - Quick Reference

A quick command reference for presenters and practitioners.

## Essential Commands

### Installation
```bash
# Install the extension
gh extension install github/gh-actions-importer

# Check version
gh actions-importer version

# Update to latest
gh extension upgrade gh-actions-importer
```

### Configuration
```bash
# Interactive configuration
gh actions-importer configure

# Configure for Jenkins
gh actions-importer configure jenkins

# Test configuration
gh actions-importer audit jenkins --list-jobs
```

### Audit
```bash
# Basic audit
gh actions-importer audit jenkins --output-dir ./audit

# Audit specific folder
gh actions-importer audit jenkins \
  --source-url http://jenkins/job/folder \
  --output-dir ./audit

# List all jobs (quick check)
gh actions-importer audit jenkins --list-jobs
```

### Forecast
```bash
# Forecast for last 30 days (default)
gh actions-importer forecast jenkins --output-dir ./forecast

# Forecast for specific time period
gh actions-importer forecast jenkins \
  --start-date 2024-01-01 \
  --end-date 2024-01-31 \
  --output-dir ./forecast

# Forecast for specific job
gh actions-importer forecast jenkins \
  --source-url http://jenkins/job/my-job \
  --output-dir ./forecast
```

### Dry Run
```bash
# Dry run for a single job
gh actions-importer dry-run jenkins \
  --source-url http://jenkins/job/my-job \
  --output-dir ./dry-run

# Dry run with custom config
gh actions-importer dry-run jenkins \
  --source-url http://jenkins/job/my-job \
  --config-file custom-config.yml \
  --output-dir ./dry-run
```

### Migrate
```bash
# Migrate and create PR
gh actions-importer migrate jenkins \
  --source-url http://jenkins/job/my-job \
  --target-url https://github.com/org/repo \
  --output-dir ./migration

# Migrate without creating PR (local only)
gh actions-importer migrate jenkins \
  --source-url http://jenkins/job/my-job \
  --target-url https://github.com/org/repo \
  --output-dir ./migration \
  --no-pull-request
```

## Common Options

### Global Flags
```bash
--help              # Show help for any command
--version           # Show version information
-v, --verbose       # Enable verbose logging
--no-telemetry      # Disable telemetry
```

### Source Options
```bash
--source-url URL              # Jenkins job URL
--jenkins-username USER       # Jenkins username
--jenkins-access-token TOKEN  # Jenkins API token
--jenkins-instance-url URL    # Jenkins base URL
```

### Target Options
```bash
--target-url URL              # GitHub repository URL
--github-access-token TOKEN   # GitHub PAT
--github-instance-url URL     # GitHub instance (for GHES)
```

### Output Options
```bash
--output-dir PATH    # Directory for output files
--config-file PATH   # Custom configuration file
--no-pull-request    # Don't create PR (local only)
```

## Environment Variables

Alternative to interactive configuration:

```bash
# Jenkins configuration
export JENKINS_USERNAME="your-username"
export JENKINS_ACCESS_TOKEN="your-token"
export JENKINS_INSTANCE_URL="http://jenkins"

# GitHub configuration
export GITHUB_ACCESS_TOKEN="ghp_your_token"
export GITHUB_INSTANCE_URL="https://github.com"

# Then run commands without interactive prompts
gh actions-importer audit jenkins --output-dir ./audit
```

## Jenkins Job URL Patterns

```bash
# Simple job
http://jenkins/job/my-job

# Job in folder
http://jenkins/job/folder/job/my-job

# Multibranch pipeline
http://jenkins/job/my-repo

# Organization folder
http://jenkins/job/my-org
```

## Output Files

After running commands, expect these files:

### Audit Output
```
audit/
├── audit-summary.md          # High-level summary
├── audit-details.json        # Detailed JSON data
├── plugins.md                # Plugin analysis
└── jobs/                     # Individual job audits
    ├── job1-audit.md
    └── job2-audit.md
```

### Forecast Output
```
forecast/
├── forecast.md               # Usage projections
├── forecast.json             # Raw data
└── charts/                   # Usage charts (if available)
```

### Dry Run Output
```
dry-run/
└── my-job/
    ├── .github/
    │   └── workflows/
    │       └── my-job.yml    # Converted workflow
    └── audit.md              # Migration notes
```

### Migration Output
```
migration/
└── my-job/
    ├── .github/
    │   └── workflows/
    │       └── my-job.yml    # Converted workflow
    ├── audit.md              # Migration notes
    └── pr-url.txt            # PR URL
```

## Jenkinsfile to GitHub Actions Mapping

Quick reference for common conversions:

| Jenkins | GitHub Actions |
|---------|---------------|
| `pipeline { }` | `name:` + `on:` + `jobs:` |
| `agent any` | `runs-on: ubuntu-latest` |
| `agent { docker }` | `container:` |
| `stages { }` | `jobs:` |
| `stage('Name')` | `job-id:` |
| `steps { }` | `steps:` |
| `sh 'command'` | `run: command` |
| `checkout scm` | `uses: actions/checkout@v4` |
| `when { branch }` | `if: github.ref == 'refs/heads/main'` |
| `environment { }` | `env:` |
| `credentials()` | `${{ secrets.NAME }}` |
| `parallel { }` | Multiple jobs or matrix |
| `post { always }` | `if: always()` |
| `input { }` | GitHub Environments with reviewers |

## Common GitHub Actions

Replace Jenkins plugins with these actions:

| Purpose | Action |
|---------|--------|
| Checkout code | `actions/checkout@v4` |
| Setup Node.js | `actions/setup-node@v4` |
| Setup Python | `actions/setup-python@v4` |
| Setup Java | `actions/setup-java@v4` |
| Setup Go | `actions/setup-go@v4` |
| Cache dependencies | Automatic with setup actions |
| Docker build/push | `docker/build-push-action@v5` |
| Docker login | `docker/login-action@v3` |
| Upload artifacts | `actions/upload-artifact@v4` |
| Download artifacts | `actions/download-artifact@v4` |
| Slack notification | `slackapi/slack-github-action@v1` |
| Test reporting | `EnricoMi/publish-unit-test-result-action@v2` |

## Troubleshooting Commands

```bash
# Check if Docker is running
docker ps

# Check GitHub CLI authentication
gh auth status

# Test Jenkins connectivity
curl -u username:token http://jenkins/api/json

# View importer logs
gh actions-importer audit jenkins -v --output-dir ./audit

# Clear Docker cache if issues
docker system prune -a

# Reinstall importer
gh extension remove gh-actions-importer
gh extension install github/gh-actions-importer
```

## Demo Setup Checklist

Before presenting:

- [ ] Docker installed and running
- [ ] GitHub CLI installed (`gh --version`)
- [ ] GitHub Actions Importer installed
- [ ] Jenkins credentials ready (username + token)
- [ ] GitHub PAT ready (with `workflow` scope)
- [ ] Test Jenkins connectivity
- [ ] Test GitHub authentication
- [ ] Prepare sample Jenkinsfiles
- [ ] Create demo repositories
- [ ] Run through demos once
- [ ] Prepare backup examples
- [ ] Have troubleshooting guide ready

## Time-Saving Tips

1. **Pre-configure credentials** before the demo
   ```bash
   gh actions-importer configure
   ```

2. **Pre-run audit** and save output to show faster results
   ```bash
   gh actions-importer audit jenkins --output-dir ./demo-audit
   ```

3. **Use small test jobs** for faster demonstrations

4. **Keep examples simple** initially, show complex ones later

5. **Have URLs ready** in a text file to copy-paste quickly

6. **Use terminal history** - prepare commands in advance

7. **Split terminal** - show output and commands side-by-side

8. **Pre-create repositories** for migration demos

## Q&A Preparation

Have answers ready for:

1. **"How much does GitHub Actions cost?"**
   - Free tier: 2,000 minutes/month for private repos
   - Team: $4/user + included minutes
   - Enterprise: Custom pricing
   - Show forecast report for estimates

2. **"What about scripted pipelines?"**
   - Cannot be automatically converted
   - Manual rewrite required
   - Good opportunity to modernize
   - Show before/after example

3. **"How do we migrate secrets?"**
   - Manual process for security
   - Use GitHub Secrets or Key Vault
   - Document mapping
   - Rotate during migration

4. **"Can we run both Jenkins and Actions?"**
   - Yes! Recommended approach
   - Run in parallel during validation
   - Gradual cutover
   - Keep Jenkins as backup

5. **"What about self-hosted runners?"**
   - Available for GitHub Actions
   - Same performance as Jenkins agents
   - Better integration with GitHub
   - Easier to manage

## Additional Resources

- [Official Migration Guide](https://docs.github.com/en/actions/migrating-to-github-actions)
- [Actions Syntax Reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Actions Marketplace](https://github.com/marketplace?type=actions)
- [GitHub Community](https://github.community/)

---

**Tip**: Print this guide and keep it handy during your presentation!
