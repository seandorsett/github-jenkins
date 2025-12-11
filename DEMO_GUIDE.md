# Jenkins to GitHub Actions Migration - Demo Guide

**Duration:** 60 minutes (Presentation + Demos)  
**Audience:** Beginner knowledge of GitHub Workflows, Expert knowledge of Jenkins  
**Tool:** GitHub Actions Importer

## Overview

This guide contains 5 progressive demos showcasing automated migration from Jenkins to GitHub Actions. Each demo builds on the previous one, taking you from initial setup through complete migration.

## Prerequisites

Before starting the demos, ensure you have:
- Jenkins instance with API access
- GitHub account with repository access
- Docker installed locally
- GitHub CLI (`gh`) installed
- Personal Access Tokens ready:
  - Jenkins API token with read permissions
  - GitHub PAT with `workflow` scope

## Demo Flow Timeline

| Demo | Topic | Duration | Key Takeaways |
|------|-------|----------|---------------|
| 1 | Setup & Configuration | 10 min | Install tools, configure credentials |
| 2 | Audit Your Pipelines | 10 min | Understand your Jenkins landscape |
| 3 | Forecast Usage & Costs | 10 min | Plan for GitHub Actions usage |
| 4 | Dry Run Migration | 12 min | Preview migration output |
| 5 | Execute Full Migration | 13 min | Complete the migration |
| Q&A | Questions | 5 min | Address audience questions |

---

## Demo 1: Setup & Configuration (10 minutes)

### Objective
Install GitHub Actions Importer and configure it to connect to both Jenkins and GitHub.

### Talking Points
- GitHub Actions Importer is a CLI extension for the GitHub CLI
- It uses Docker containers to perform the migration work
- Credentials are stored securely and used only during migration

### Demo Script

#### Step 1: Verify Prerequisites (2 min)

```bash
# Verify Docker is running
docker --version
# Expected: Docker version 24.x or higher

# Verify GitHub CLI is installed
gh --version
# Expected: gh version 2.x or higher
```

**Presenter Notes:** Explain that Docker is required because the importer runs in containers to ensure consistent behavior across environments.

#### Step 2: Install GitHub Actions Importer (3 min)

```bash
# Install the gh-actions-importer extension
gh extension install github/gh-actions-importer

# Verify installation
gh actions-importer version

# View available commands
gh actions-importer --help
```

**Expected Output:**
```
Available commands:
  audit      - Audit your CI/CD pipelines
  configure  - Configure credentials for GitHub Actions Importer
  dry-run    - Convert a pipeline to a GitHub Actions workflow
  forecast   - Forecast GitHub Actions usage
  migrate    - Convert a pipeline and open a pull request
```

**Presenter Notes:** Walk through each command briefly, explaining that you'll demonstrate each one in the subsequent demos.

#### Step 3: Configure Credentials (5 min)

```bash
# Start the configuration wizard
gh actions-importer configure
```

**Interactive Prompts:**
```
? Which CI/CD platform are you migrating from?
  > jenkins

? GitHub Personal Access Token (with workflow scope):
  > [Enter your GitHub PAT]

? GitHub URL (press enter for default: https://github.com):
  > [Press Enter]

? Jenkins access token:
  > [Enter your Jenkins API token]

? Jenkins username:
  > [Enter your Jenkins username]

? Jenkins URL:
  > http://your-jenkins-server.com

✓ Credentials configured successfully
```

**Presenter Notes:** 
- Mention that tokens are stored securely in Docker volumes
- Explain that the GitHub token needs `workflow` scope to create pull requests
- Jenkins token needs read access to jobs and build history
- You can also provide credentials via environment variables for automation

**Key Takeaways:**
- ✅ GitHub Actions Importer is easy to install
- ✅ Configuration is interactive and user-friendly
- ✅ Credentials are stored securely
- ✅ Ready to analyze Jenkins pipelines

---

## Demo 2: Audit Your Pipelines (10 minutes)

### Objective
Run an audit to inventory all Jenkins jobs and understand migration readiness.

### Talking Points
- Audit provides a comprehensive overview of your Jenkins environment
- Identifies which pipelines can be automatically migrated
- Highlights manual work needed (secrets, custom plugins, etc.)
- Generates a detailed report for planning

### Demo Script

#### Step 1: Run Basic Audit (4 min)

```bash
# Run audit against your Jenkins instance
gh actions-importer audit jenkins --output-dir ./audit-results

# The command analyzes all accessible jobs
```

**Expected Output:**
```
[INFO] Starting audit of Jenkins instance
[INFO] Fetching job configurations...
[INFO] Analyzing 47 jobs...
[INFO] Generating audit report...
[INFO] Audit complete!

Report saved to: ./audit-results/audit-summary.md
```

**Presenter Notes:** Explain that the audit runs in the background, analyzing job configurations, build history, and plugin usage.

#### Step 2: Review Audit Summary (6 min)

```bash
# View the audit summary
cat ./audit-results/audit-summary.md
```

**Example Audit Summary:**
```markdown
# Jenkins Audit Summary

## Pipeline Statistics
- Total Jobs: 47
- Declarative Pipelines: 32
- Scripted Pipelines: 15
- Freestyle Jobs: 0

## Migration Readiness
- ✅ Automatically Migratable: 32 (68%)
- ⚠️  Requires Manual Work: 15 (32%)

## Common Manual Tasks Required
1. **Secrets Migration** (47 jobs)
   - Migrate credentials from Jenkins to GitHub Secrets
   
2. **Custom Plugins** (12 jobs)
   - docker-workflow (8 jobs)
   - kubernetes (4 jobs)
   - Action equivalents available
   
3. **Scripted Pipelines** (15 jobs)
   - Cannot be automatically converted
   - Manual rewrite recommended

## Recommended Actions
1. Start with declarative pipelines for quick wins
2. Migrate secrets to GitHub repository secrets
3. Review custom plugin mappings in the detailed report
4. Plan manual migration for scripted pipelines
```

**Presenter Notes:**
- Walk through each section of the report
- Emphasize the 68% automatic migration rate
- Explain that scripted pipelines require manual work because they're more complex
- Show that the tool identifies specific plugins and suggests GitHub Actions equivalents

**Key Takeaways:**
- ✅ Quick visibility into entire Jenkins landscape
- ✅ Clear migration roadmap
- ✅ Identifies blockers early
- ✅ Helps prioritize migration order

---

## Demo 3: Forecast Usage & Costs (10 minutes)

### Objective
Estimate GitHub Actions usage and costs based on historical Jenkins build data.

### Talking Points
- Forecasting helps with capacity planning
- Uses historical build duration data from Jenkins
- Provides cost estimates for GitHub Actions
- Helps compare costs between Jenkins and GitHub Actions
- Useful for getting budget approval

### Demo Script

#### Step 1: Run Forecast (3 min)

```bash
# Generate forecast based on last 30 days of builds
gh actions-importer forecast jenkins --output-dir ./forecast-results
```

**Expected Output:**
```
[INFO] Fetching build history for the last 30 days...
[INFO] Analyzing 1,247 builds across 47 jobs...
[INFO] Calculating GitHub Actions usage metrics...
[INFO] Generating forecast report...
[INFO] Forecast complete!

Report saved to: ./forecast-results/forecast.md
```

**Presenter Notes:** Explain that the forecast analyzes actual build durations and maps them to GitHub Actions runner types.

#### Step 2: Review Forecast Report (7 min)

```bash
# View the forecast
cat ./forecast-results/forecast.md
```

**Example Forecast Report:**
```markdown
# GitHub Actions Usage Forecast

## Analysis Period
- Start Date: 2024-11-11
- End Date: 2024-12-11
- Total Builds Analyzed: 1,247

## Monthly Usage Projection

### Compute Minutes by Runner Type
- **Ubuntu Latest**: 34,500 minutes/month
- **Windows Latest**: 8,200 minutes/month  
- **MacOS Latest**: 2,100 minutes/month
- **Total**: 44,800 minutes/month

### Cost Estimate (GitHub Team Plan)
- Ubuntu: 34,500 min × $0.008/min = $276.00
- Windows: 8,200 min × $0.016/min = $131.20
- MacOS: 2,100 min × $0.08/min = $168.00
- **Total Monthly Cost**: $575.20

### Peak Usage Times
- Highest activity: Monday-Friday, 9:00 AM - 5:00 PM
- Peak concurrent jobs: 12
- Recommended concurrent runners: 15 (with 20% buffer)

## Comparison with Current Jenkins Setup
- Current Jenkins infrastructure cost: ~$1,200/month (estimated)
- GitHub Actions projected cost: $575.20/month
- **Potential Savings**: $624.80/month (52%)

## Recommendations
1. Start with GitHub Team plan (3,000 free minutes included)
2. Monitor actual usage during first month
3. Consider self-hosted runners for high-volume jobs
4. Implement caching to reduce build times by 30-40%
```

**Presenter Notes:**
- Highlight the cost savings compared to Jenkins infrastructure
- Explain GitHub Actions pricing tiers
- Mention that free tier includes 2,000 minutes/month for private repos
- Discuss self-hosted runners as an option for cost optimization
- Note that caching and other optimizations can further reduce costs

**Key Takeaways:**
- ✅ Data-driven cost planning
- ✅ Realistic usage projections
- ✅ Helps with budgeting and approval
- ✅ Identifies optimization opportunities

---

## Demo 4: Dry Run Migration (12 minutes)

### Objective
Convert a sample Jenkins pipeline to GitHub Actions workflow without making any changes to repositories.

### Talking Points
- Dry run lets you preview the migration output
- No changes are made to your repositories
- Perfect for understanding what the migration will produce
- Allows you to test and iterate before committing
- Identifies issues that need manual attention

### Demo Script

#### Step 1: Choose a Sample Pipeline (2 min)

**Presenter Notes:** Explain that you'll start with a simple declarative pipeline to show a successful migration first.

```bash
# List available Jenkins jobs
gh actions-importer audit jenkins --list-jobs

# Select a simple declarative pipeline for the demo
# We'll use "backend-api-build" as an example
```

#### Step 2: Review Source Jenkinsfile (3 min)

```groovy
// Jenkinsfile for backend-api-build
pipeline {
    agent any
    
    environment {
        NODE_ENV = 'production'
        API_KEY = credentials('api-key-secret')
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('Run Tests') {
            steps {
                sh 'npm test'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }
        
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh './deploy.sh'
            }
        }
    }
    
    post {
        always {
            junit 'test-results/**/*.xml'
        }
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
```

**Presenter Notes:** Walk through the Jenkinsfile, highlighting:
- Declarative syntax
- Environment variables and secrets
- Multiple stages
- Conditional execution
- Post-build actions

#### Step 3: Run Dry Run (2 min)

```bash
# Perform dry run for the selected pipeline
gh actions-importer dry-run jenkins \
  --source-url http://your-jenkins-server.com/job/backend-api-build \
  --output-dir ./dry-run-results
```

**Expected Output:**
```
[INFO] Fetching pipeline configuration...
[INFO] Converting pipeline to GitHub Actions workflow...
[INFO] Analyzing plugins and dependencies...
[INFO] Generating workflow file...
[INFO] Dry run complete!

Workflow saved to: ./dry-run-results/backend-api-build/.github/workflows/backend-api-build.yml
Audit saved to: ./dry-run-results/backend-api-build/audit.md
```

#### Step 4: Review Generated Workflow (5 min)

```bash
# View the generated GitHub Actions workflow
cat ./dry-run-results/backend-api-build/.github/workflows/backend-api-build.yml
```

**Generated Workflow:**
```yaml
name: Backend API Build

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

env:
  NODE_ENV: production

jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
      
      - name: Install Dependencies
        run: npm install
      
      - name: Run Tests
        run: npm test
      
      - name: Build
        run: npm run build
      
      - name: Deploy
        if: github.ref == 'refs/heads/main'
        env:
          API_KEY: ${{ secrets.API_KEY }}
        run: ./deploy.sh
      
      - name: Publish Test Results
        if: always()
        uses: EnricoMi/publish-unit-test-result-action@v2
        with:
          files: test-results/**/*.xml
```

**Presenter Notes:** Compare the GitHub Actions workflow with the original Jenkinsfile:
- **Triggers**: Push and pull_request events replace SCM polling
- **Environment**: Global env and job-level env variables
- **Secrets**: Migrated from Jenkins credentials to GitHub Secrets
- **Steps**: Direct mapping from Jenkins stages
- **Conditions**: `when` becomes `if` with GitHub expressions
- **Post-actions**: `if: always()` for test results

#### Review Migration Notes

```bash
# Check the audit file for migration notes
cat ./dry-run-results/backend-api-build/audit.md
```

**Example Audit Notes:**
```markdown
# Migration Audit: backend-api-build

## ✅ Successfully Converted
- All stages mapped to workflow steps
- Environment variables converted
- Conditional execution preserved
- Post-build actions converted

## ⚠️ Manual Actions Required

### 1. Secrets Migration
- `api-key-secret` must be added to GitHub Secrets
- Navigate to Settings > Secrets and variables > Actions
- Add secret with name: `API_KEY`

### 2. Recommended Improvements
- Add caching for node_modules (already added via setup-node)
- Consider using matrix strategy for testing multiple Node versions
- Add artifact upload for build outputs

## 📊 Migration Fidelity
- Automatic Conversion: 95%
- Manual Work Required: 5%
- Confidence Level: High
```

**Presenter Notes:**
- Emphasize that the migration is 95% automatic
- Show that the tool identifies what manual work is needed
- Explain that secrets must be manually migrated for security reasons
- Mention the recommendations for improving the workflow

**Key Takeaways:**
- ✅ Preview migration before committing
- ✅ High fidelity conversion
- ✅ Clear guidance on manual steps
- ✅ Optimization recommendations included

---

## Demo 5: Execute Full Migration (13 minutes)

### Objective
Perform the actual migration, create a pull request, and validate the workflow.

### Talking Points
- This is where the magic happens
- Importer creates a pull request with the new workflow
- Allows for review before merging
- Can run in parallel with Jenkins for validation
- Easy rollback if needed

### Demo Script

#### Step 1: Prepare Repository (2 min)

```bash
# Ensure you have a target GitHub repository
# Create one if needed or use existing

# Clone the repository
git clone https://github.com/your-org/backend-api.git
cd backend-api

# Verify repository is clean
git status
```

**Presenter Notes:** Explain that the migration will create a new branch and open a PR, so the repository can be in any state.

#### Step 2: Run Migration Command (3 min)

```bash
# Execute the migration
gh actions-importer migrate jenkins \
  --source-url http://your-jenkins-server.com/job/backend-api-build \
  --target-url https://github.com/your-org/backend-api \
  --output-dir ./migration-results
```

**Expected Output:**
```
[INFO] Fetching pipeline configuration...
[INFO] Converting pipeline to GitHub Actions workflow...
[INFO] Creating branch: actions-importer/backend-api-build
[INFO] Committing workflow file...
[INFO] Pushing changes to GitHub...
[INFO] Creating pull request...

✓ Migration complete!

Pull Request: https://github.com/your-org/backend-api/pull/42
Branch: actions-importer/backend-api-build
```

**Presenter Notes:** 
- Show that the process is automated end-to-end
- Explain that a new branch is created for safety
- Mention that the PR description includes migration notes

#### Step 3: Review Pull Request (4 min)

Navigate to the pull request URL in your browser.

**PR Description (Auto-generated):**
```markdown
# Migrate backend-api-build from Jenkins to GitHub Actions

This pull request was automatically generated by GitHub Actions Importer.

## Changes
- Added `.github/workflows/backend-api-build.yml`
- Converted Jenkins pipeline to GitHub Actions workflow

## Migration Summary
- **Conversion Rate**: 95%
- **Manual Actions Required**: See checklist below

## ✅ Pre-Merge Checklist
- [ ] Add `API_KEY` to repository secrets
- [ ] Review workflow triggers (push, pull_request)
- [ ] Verify environment variables
- [ ] Test workflow on a feature branch
- [ ] Update team documentation

## 📝 Migration Notes
See the workflow file for detailed comments on the conversion.

## 🔄 Validation Plan
1. Merge this PR
2. Run workflow manually via Actions tab
3. Verify build succeeds
4. Compare output with Jenkins build
5. Run parallel builds (Jenkins + Actions) for 1 week

## 🆘 Rollback
If issues arise, you can:
- Revert this PR
- Continue using Jenkins
- Address issues and retry

---
*Generated by GitHub Actions Importer*
```

**Presenter Notes:**
- Walk through the PR description
- Show the files changed tab
- Review the workflow file in the diff view
- Highlight the helpful checklist
- Mention that you can make additional changes if needed

#### Step 4: Configure Secrets (2 min)

```bash
# Add required secrets via GitHub CLI
gh secret set API_KEY --body "your-api-key-value" --repo your-org/backend-api

# Verify secret was added
gh secret list --repo your-org/backend-api
```

**Expected Output:**
```
API_KEY    Updated 2024-12-11
```

**Presenter Notes:** Explain that secrets can also be added via the web interface at Settings > Secrets and variables > Actions.

#### Step 5: Test the Workflow (2 min)

```bash
# Merge the pull request
gh pr merge 42 --squash --repo your-org/backend-api

# View workflow runs
gh run list --repo your-org/backend-api
```

**Expected Output:**
```
STATUS  NAME                  WORKFLOW              BRANCH  EVENT  ID          
✓       Backend API Build     backend-api-build.yml main    push   1234567890
```

```bash
# Watch the workflow run in real-time
gh run watch 1234567890 --repo your-org/backend-api
```

**Expected Output:**
```
✓ main Backend API Build · 1234567890
Triggered via push about 1 minute ago

JOBS
✓ build (ubuntu-latest) in 2m 34s
  ✓ Set up job
  ✓ Checkout
  ✓ Setup Node.js
  ✓ Install Dependencies
  ✓ Run Tests
  ✓ Build
  ✓ Deploy
  ✓ Publish Test Results
  ✓ Complete job

✓ Successful in 2m 45s
```

**Presenter Notes:**
- Show that the workflow ran successfully
- Compare the execution time with Jenkins (often faster due to caching)
- Point out the clear step-by-step output
- Mention that logs are accessible and searchable

#### Step 6: Validation & Next Steps (1 min)

**Presenter Notes:** Conclude with validation strategy:

```markdown
## Validation Strategy
1. ✅ First workflow run successful
2. Run parallel builds (Jenkins + GitHub Actions) for 1-2 weeks
3. Compare outputs, timing, and reliability
4. Gradually shift traffic to GitHub Actions
5. Decommission Jenkins pipeline once confident

## Benefits Realized
- ✅ Infrastructure as code (workflow in repository)
- ✅ Better visibility (Actions UI)
- ✅ Integrated with GitHub (PR checks, status badges)
- ✅ Faster builds (caching, matrix builds)
- ✅ Cost savings (pay-per-use model)
```

**Key Takeaways:**
- ✅ End-to-end automated migration
- ✅ Safe with PR review process
- ✅ Easy to validate before full cutover
- ✅ Clear path to production

---

## Summary & Best Practices

### Migration Best Practices

1. **Start Small**
   - Begin with simple, low-risk pipelines
   - Build confidence with quick wins
   - Learn the tools before tackling complex pipelines

2. **Run in Parallel**
   - Keep Jenkins running during migration
   - Compare outputs between systems
   - Ensure parity before decommissioning

3. **Migrate Secrets Carefully**
   - Use GitHub Secrets or Azure Key Vault
   - Rotate credentials during migration
   - Document secret mappings

4. **Leverage GitHub Features**
   - Use matrix builds for testing multiple versions
   - Implement caching to speed up builds
   - Use reusable workflows for common patterns

5. **Document Everything**
   - Update team documentation
   - Create runbooks for new workflows
   - Train team on GitHub Actions

### Common Pitfalls to Avoid

❌ **Don't:**
- Migrate everything at once (too risky)
- Ignore the audit report (manual work items)
- Forget to test workflows before merging
- Skip secrets migration planning
- Delete Jenkins immediately after migration

✅ **Do:**
- Migrate incrementally
- Address all manual action items
- Test thoroughly in non-production first
- Plan secrets migration in advance
- Keep Jenkins as backup for 2-4 weeks

### Post-Migration Checklist

- [ ] All pipelines successfully migrated
- [ ] Secrets configured in GitHub
- [ ] Team trained on GitHub Actions
- [ ] Documentation updated
- [ ] Monitoring and alerts configured
- [ ] Cost tracking enabled
- [ ] Jenkins instance backed up
- [ ] Decommission plan approved

---

## Q&A Topics

Be prepared to answer questions about:

1. **Cost Comparison**
   - GitHub Actions vs. Jenkins infrastructure costs
   - Self-hosted runners option
   - Optimization strategies

2. **Enterprise Features**
   - GitHub Enterprise policies
   - Required reviewers for deployments
   - Audit logging and compliance

3. **Advanced Scenarios**
   - Matrix builds for multiple environments
   - Reusable workflows
   - Custom actions
   - Self-hosted runners

4. **Limitations**
   - What can't be automatically migrated
   - Plugin equivalents in GitHub Actions
   - Migration of scripted pipelines

5. **Security**
   - Secrets management
   - OIDC for cloud deployments
   - Branch protection rules

---

## Additional Resources

### Documentation
- [GitHub Actions Documentation](https://docs.github.com/actions)
- [GitHub Actions Importer](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/automating-migration-with-github-actions-importer)
- [Jenkins Migration Guide](https://docs.github.com/en/actions/migrating-to-github-actions/automated-migrations/migrating-from-jenkins-with-github-actions-importer)

### Tools
- [GitHub CLI](https://cli.github.com/)
- [Actions Marketplace](https://github.com/marketplace?type=actions)
- [Workflow Syntax Reference](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

### Community
- [GitHub Community](https://github.community/)
- [GitHub Actions Discussions](https://github.com/orgs/community/discussions/categories/actions)
- [Awesome Actions](https://github.com/sdras/awesome-actions)

---

## Appendix: Sample Jenkinsfiles

See the `demos/` directory for complete sample Jenkinsfiles used in these demos.

- `demos/demo4-simple-pipeline/` - Basic Node.js build pipeline
- `demos/demo4-complex-pipeline/` - Advanced pipeline with parallel stages
- `demos/demo5-monorepo/` - Monorepo with multiple services

---

**Demo Guide Version:** 1.0  
**Last Updated:** December 2024  
**Maintained by:** Your Organization's DevOps Team
