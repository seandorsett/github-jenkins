# Demo 5: Monorepo Pipeline Example

## Overview
This example demonstrates a monorepo setup with multiple services, showing how to detect changes and build only affected services.

## Monorepo Structure
```
.
├── services/
│   ├── api/
│   ├── web/
│   ├── worker/
│   └── admin/
├── shared/
│   └── common libraries
└── Jenkinsfile
```

## Pipeline Features
- ✅ Change detection (build only what changed)
- ✅ Dynamic parallel execution
- ✅ Selective service building
- ✅ Shared library handling
- ✅ Integration testing with docker-compose
- ✅ Kubernetes deployments
- ✅ Smart triggering based on paths

## Migration Expectations

### GitHub Actions Advantages for Monorepos
GitHub Actions has **better** built-in support for monorepos:

1. **Path Filters**: Native support for triggering workflows based on file paths
2. **Matrix Builds**: Better syntax for building multiple services
3. **Reusable Workflows**: Share common build logic across services
4. **Change Detection**: Built-in `paths` and `paths-ignore` filters

### Migration Approach

Instead of one complex Jenkinsfile, you'll have:
- Multiple workflow files (one per service or group)
- Path-based triggers
- Reusable workflow for common steps
- Matrix strategy for parallel builds

### Example Structure After Migration
```
.github/
├── workflows/
│   ├── api.yml           # Triggers on services/api/** changes
│   ├── web.yml           # Triggers on services/web/** changes
│   ├── worker.yml        # Triggers on services/worker/** changes
│   ├── shared.yml        # Triggers on shared/** changes
│   └── integration.yml   # Runs after service builds
└── actions/
    └── build-node-service/  # Reusable action for Node.js builds
        └── action.yml
```

## Sample Converted Workflow (api.yml)

```yaml
name: API Service

on:
  push:
    paths:
      - 'services/api/**'
      - 'shared/**'
    branches:
      - main
      - develop
  pull_request:
    paths:
      - 'services/api/**'
      - 'shared/**'

jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: services/api
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'
          cache-dependency-path: services/api/package-lock.json
      
      - name: Install Dependencies
        run: npm install
      
      - name: Build
        run: npm run build
      
      - name: Test
        run: npm test
      
      - name: Deploy to Kubernetes
        if: github.ref == 'refs/heads/main'
        run: |
          kubectl apply -f k8s/
          kubectl rollout status deployment/api
```

## Migration Fidelity: 85%

### What Improves
- ✅ Cleaner path-based triggering
- ✅ Better parallel execution with matrix
- ✅ Reusable workflows reduce duplication
- ✅ Each service can have its own workflow

### Manual Work Required
1. Split monolithic Jenkinsfile into service-specific workflows
2. Set up reusable workflows for common patterns
3. Configure path filters for each workflow
4. Update deployment scripts for GitHub Actions context
5. Set up workflow dependencies (if needed)

## Benefits of GitHub Actions for Monorepos

1. **Better Isolation**: Each service has its own workflow
2. **Faster CI**: Only affected services build
3. **Clearer History**: See which service failed at a glance
4. **Easier Maintenance**: Update one service's CI without affecting others
5. **Better Caching**: Per-service cache keys

## Migration Time
- Initial split: 1-2 hours
- Reusable workflow setup: 1 hour
- Testing & validation: 2-3 hours
- **Total**: 4-6 hours for full monorepo

## Best Practices
1. Use `paths` filters to trigger only relevant builds
2. Create reusable workflows for common build patterns
3. Use matrix strategy for building similar services
4. Implement change detection to skip unchanged services
5. Set up workflow dependencies with `needs`
6. Use artifacts to share build outputs between workflows
