# Troubleshooting Guide - GitHub Actions Importer

Common issues and solutions when migrating from Jenkins to GitHub Actions.

## Installation Issues

### Problem: Cannot install gh extension
```
Error: extension not found
```

**Solution:**
```bash
# Update GitHub CLI first
brew upgrade gh  # macOS
# or
sudo apt update && sudo apt upgrade gh  # Ubuntu

# Then install extension
gh extension install github/gh-actions-importer
```

### Problem: Docker not found
```
Error: docker command not found
```

**Solution:**
Install Docker Desktop or Docker Engine:
- macOS: [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Windows: [Docker Desktop](https://www.docker.com/products/docker-desktop)
- Linux: `sudo apt install docker.io` or similar

Verify: `docker --version`

### Problem: Docker daemon not running
```
Error: Cannot connect to Docker daemon
```

**Solution:**
```bash
# Start Docker Desktop (macOS/Windows)
# or start Docker service (Linux)
sudo systemctl start docker

# Verify
docker ps
```

## Configuration Issues

### Problem: Jenkins authentication fails
```
Error: 401 Unauthorized
```

**Solutions:**

1. **Check credentials:**
   ```bash
   # Test Jenkins API manually
   curl -u username:token http://jenkins/api/json
   ```

2. **Generate new API token:**
   - Jenkins → User → Configure → API Token
   - Generate new token
   - Reconfigure importer: `gh actions-importer configure`

3. **Check user permissions:**
   - User needs read access to jobs
   - User needs access to build history

### Problem: GitHub authentication fails
```
Error: Bad credentials
```

**Solutions:**

1. **Verify GitHub CLI authentication:**
   ```bash
   gh auth status
   ```

2. **Re-authenticate:**
   ```bash
   gh auth login
   ```

3. **Check token scopes:**
   - Token needs `workflow` scope
   - Generate new PAT with correct scopes
   - Settings → Developer settings → Personal access tokens

### Problem: Cannot connect to Jenkins
```
Error: Connection refused
```

**Solutions:**

1. **Check Jenkins URL:**
   - Include protocol: `http://` or `https://`
   - No trailing slash
   - Verify in browser first

2. **Check network access:**
   - VPN required?
   - Firewall blocking?
   - Use `curl` to test: `curl -I http://jenkins`

3. **Check Jenkins availability:**
   ```bash
   ping jenkins.example.com
   curl http://jenkins/api/json
   ```

## Audit Issues

### Problem: No jobs found
```
Warning: No jobs found to audit
```

**Solutions:**

1. **Check permissions:**
   - User has read access to jobs?
   - Try accessing jobs in browser

2. **Verify URL:**
   ```bash
   # List jobs to verify connectivity
   gh actions-importer audit jenkins --list-jobs
   ```

3. **Check folder path:**
   - For jobs in folders: `http://jenkins/job/folder/job/job-name`
   - For multibranch: `http://jenkins/job/repo-name`

### Problem: Audit times out
```
Error: Request timeout
```

**Solutions:**

1. **Audit smaller subsets:**
   ```bash
   # Audit specific folder instead of all jobs
   gh actions-importer audit jenkins \
     --source-url http://jenkins/job/team1 \
     --output-dir ./audit
   ```

2. **Increase timeout (if supported):**
   - Check importer version for timeout options
   - Break into smaller audits

### Problem: Some jobs missing from audit
```
Warning: X jobs could not be processed
```

**Solutions:**

1. **Check job types:**
   - Disabled jobs may be skipped
   - Check Jenkins job status

2. **Check permissions:**
   - User may not have access to all jobs
   - Review with Jenkins admin

3. **Review logs:**
   ```bash
   gh actions-importer audit jenkins -v --output-dir ./audit
   ```

## Forecast Issues

### Problem: No build history
```
Warning: No builds found for forecasting
```

**Solutions:**

1. **Check date range:**
   ```bash
   # Increase date range
   gh actions-importer forecast jenkins \
     --start-date 2024-01-01 \
     --end-date 2024-12-31 \
     --output-dir ./forecast
   ```

2. **Verify builds exist:**
   - Check Jenkins build history in browser
   - May need to run jobs first

3. **Check permissions:**
   - User needs access to build history

### Problem: Forecast seems inaccurate
```
Forecasted minutes seem too high/low
```

**Solutions:**

1. **Review input data:**
   - Check which builds were analyzed
   - Look at `forecast.json` for details

2. **Consider seasonality:**
   - Last 30 days may not be representative
   - Use longer period if available

3. **Exclude outliers:**
   - Very long builds may skew data
   - Review audit report for anomalies

## Dry Run Issues

### Problem: Cannot find pipeline
```
Error: Pipeline not found
```

**Solutions:**

1. **Verify URL format:**
   ```bash
   # Correct format
   --source-url http://jenkins/job/my-job
   
   # For folders
   --source-url http://jenkins/job/folder/job/my-job
   ```

2. **Check job exists:**
   ```bash
   # List jobs first
   gh actions-importer audit jenkins --list-jobs
   ```

### Problem: Scripted pipeline conversion fails
```
Warning: Cannot automatically convert scripted pipeline
```

**Solution:**
This is expected. Scripted pipelines require manual conversion:

1. Review the scripted pipeline code
2. Manually create GitHub Actions workflow
3. Use declarative syntax where possible
4. Test thoroughly

### Problem: Generated workflow has errors
```
Warning: Manual updates required
```

**Solutions:**

1. **Review audit.md file:**
   - Lists specific manual actions needed
   - Provides recommendations

2. **Common manual fixes:**
   - Add secrets to GitHub
   - Update plugin references
   - Adjust script syntax
   - Configure environments

3. **Test locally:**
   ```bash
   # Use act to test locally
   act -W dry-run/my-job/.github/workflows/my-job.yml
   ```

## Migration Issues

### Problem: Cannot create pull request
```
Error: Failed to create pull request
```

**Solutions:**

1. **Check permissions:**
   - GitHub token needs `workflow` scope
   - User needs write access to repository

2. **Check branch protection:**
   - May need admin access to push
   - Temporary: disable branch protection

3. **Check repository:**
   ```bash
   # Verify repository exists and you have access
   gh repo view org/repo
   ```

### Problem: Branch already exists
```
Error: Branch actions-importer/job-name already exists
```

**Solutions:**

1. **Delete existing branch:**
   ```bash
   git push origin --delete actions-importer/job-name
   ```

2. **Or use different branch name:**
   - This option may not be available
   - Delete and retry

### Problem: Workflow fails after migration
```
Error: Workflow run failed
```

**Solutions:**

1. **Check secrets:**
   - All secrets migrated?
   - Secret names match?
   - Add missing secrets:
     ```bash
     gh secret set SECRET_NAME --repo org/repo
     ```

2. **Check syntax:**
   - Review workflow file
   - Use GitHub's workflow editor for validation

3. **Check permissions:**
   - Workflow may need additional permissions
   - Add to workflow:
     ```yaml
     permissions:
       contents: read
       packages: write
     ```

4. **Review logs:**
   ```bash
   gh run view --log-failed
   ```

## Common Migration Challenges

### Challenge: Custom Jenkins plugins

**Solution:**
Find GitHub Actions equivalents:

| Jenkins Plugin | GitHub Actions Alternative |
|---------------|---------------------------|
| Docker Pipeline | `docker/build-push-action@v5` |
| Kubernetes | `azure/k8s-deploy@v1` |
| AWS Steps | `aws-actions/configure-aws-credentials@v4` |
| Slack | `slackapi/slack-github-action@v1` |
| JUnit | `EnricoMi/publish-unit-test-result-action@v2` |

Search [GitHub Marketplace](https://github.com/marketplace?type=actions) for more.

### Challenge: Shared libraries

**Solution:**
Convert to reusable workflows:

```yaml
# .github/workflows/reusable-build.yml
name: Reusable Build
on:
  workflow_call:
    inputs:
      service:
        required: true
        type: string

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build ${{ inputs.service }}
        run: npm run build
```

Use it:
```yaml
# .github/workflows/api.yml
jobs:
  build:
    uses: ./.github/workflows/reusable-build.yml
    with:
      service: api
```

### Challenge: Complex scripted pipelines

**Solution:**
Manual rewrite with these patterns:

1. **Break into jobs:**
   ```yaml
   jobs:
     job1:
       runs-on: ubuntu-latest
       steps: [...]
     
     job2:
       needs: job1
       runs-on: ubuntu-latest
       steps: [...]
   ```

2. **Use composite actions for reusable steps:**
   ```yaml
   # .github/actions/my-action/action.yml
   name: My Action
   runs:
     using: composite
     steps:
       - run: echo "Reusable"
         shell: bash
   ```

3. **Convert groovy scripts to bash:**
   - Most logic can be done in bash
   - Use actions for complex operations

### Challenge: Manual approval gates

**Solution:**
Use GitHub Environments:

1. **Create environment:**
   - Settings → Environments → New environment
   - Add required reviewers

2. **Use in workflow:**
   ```yaml
   jobs:
     deploy:
       environment: production
       runs-on: ubuntu-latest
       steps:
         - name: Deploy
           run: ./deploy.sh
   ```

### Challenge: Matrix builds

**Solution:**
GitHub Actions has better matrix support:

```yaml
jobs:
  test:
    strategy:
      matrix:
        node: [14, 16, 18, 20]
        os: [ubuntu-latest, windows-latest, macos-latest]
    runs-on: ${{ matrix.os }}
    steps:
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      - run: npm test
```

## Performance Issues

### Problem: Slow migrations

**Solutions:**

1. **Audit/forecast fewer jobs:**
   - Focus on specific folders
   - Split into batches

2. **Improve Docker performance:**
   - Increase Docker memory/CPU
   - Clean up: `docker system prune -a`

3. **Network issues:**
   - Check Jenkins server performance
   - Use local Jenkins if possible

### Problem: Workflows run slower than Jenkins

**Solutions:**

1. **Add caching:**
   ```yaml
   - uses: actions/setup-node@v4
     with:
       node-version: '18'
       cache: 'npm'  # Enable caching
   ```

2. **Use matrix for parallel execution:**
   ```yaml
   strategy:
     matrix:
       test: [unit, integration, e2e]
   ```

3. **Consider self-hosted runners:**
   - Can be faster than GitHub-hosted
   - Persistent caches
   - Custom hardware

## Getting Help

If you're still stuck:

1. **Check official docs:**
   - [GitHub Actions Importer](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations)
   - [GitHub Actions](https://docs.github.com/actions)

2. **Search community:**
   - [GitHub Community](https://github.community/)
   - [Stack Overflow](https://stackoverflow.com/questions/tagged/github-actions)

3. **Enable verbose logging:**
   ```bash
   gh actions-importer audit jenkins -v --output-dir ./audit
   ```

4. **Check version:**
   ```bash
   gh actions-importer version
   # Update if needed
   gh extension upgrade gh-actions-importer
   ```

5. **File an issue:**
   - [GitHub Actions Importer Issues](https://github.com/github/gh-actions-importer/issues)
   - Include verbose logs
   - Describe Jenkins setup

## Emergency Rollback

If migration causes issues:

1. **Revert the PR:**
   ```bash
   gh pr close <PR-NUMBER>
   git push origin --delete actions-importer/job-name
   ```

2. **Keep Jenkins running:**
   - Don't decommission until confident
   - Run parallel for validation period

3. **Fix and retry:**
   - Address issues in dry run first
   - Test thoroughly before re-migrating

## Pre-Migration Checklist

Avoid issues by checking these first:

- [ ] Docker installed and running
- [ ] GitHub CLI installed and authenticated
- [ ] GitHub Actions Importer installed
- [ ] Jenkins credentials tested (API call works)
- [ ] GitHub PAT has `workflow` scope
- [ ] User has read access to all Jenkins jobs
- [ ] User has write access to GitHub repository
- [ ] Backup of Jenkins configuration
- [ ] List of secrets to migrate
- [ ] Team trained on GitHub Actions
- [ ] Rollback plan documented

---

**Remember:** Migration is a journey, not a destination. Take it slow, test thoroughly, and don't hesitate to ask for help!
