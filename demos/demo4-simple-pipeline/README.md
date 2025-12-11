# Demo 4: Simple Pipeline Example

## Overview
This directory contains a simple Node.js build pipeline used in Demo 4 of the presentation.

## Pipeline Features
- ✅ Declarative syntax (easy to migrate)
- ✅ Environment variables
- ✅ Jenkins credentials
- ✅ Conditional execution (deploy only on main)
- ✅ Post-build actions (test results, notifications)

## Migration Expectations

### Automatic Conversion
- All stages convert to workflow steps
- Environment variables map to workflow env
- Credentials map to GitHub Secrets
- Conditional logic converts to `if` expressions
- Post actions convert to conditional steps

### Manual Steps Required
1. Add `API_KEY` to GitHub Secrets
2. Review and adjust deployment script
3. Verify test results publishing

## Expected GitHub Actions Workflow

After migration, this will become `.github/workflows/backend-api-build.yml` with:
- Trigger on push and pull_request events
- Ubuntu runner
- Node.js setup with caching
- All stages as individual steps
- Conditional deployment on main branch
- Test results publishing

## Migration Fidelity: 95%
