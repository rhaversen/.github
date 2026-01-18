# Reusable GitHub Workflows

Centralized GitHub Actions workflows for all rhaversen projects.

## Available Workflows

### CI/CD (`ci-cd.yml`)

Full deployment pipeline for Node.js projects with Docker and Kubernetes.

**Usage:**
```yaml
name: CI/CD

on:
    push:
        branches: ["main", "staging"]

jobs:
    build-and-deploy:
        uses: rhaversen/.github/.github/workflows/ci-cd.yml@main
        with:
            project_name: "MyProject"
            dockerhub_image_name: "my-project"
            node_version: "lts/*"  # or "22" for specific version
        secrets:
            DOCKER_USERNAME: ${{ secrets.DOCKER_USERNAME }}
            DOCKER_PASSWORD: ${{ secrets.DOCKER_PASSWORD }}
            SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
            DEVOPS_REPO_TOKEN: ${{ secrets.DEVOPS_REPO_TOKEN }}
```

**Features:**
- Environment detection (main → production, staging → staging)
- Build and test
- Source maps upload to Sentry (production only)
- Multi-platform Docker builds (arm64, amd64)
- Automatic deployment to DevOps repo

### Development Testing - Backend (`development-backend.yml`)

PR testing pipeline for backend Node.js projects.

**Usage:**
```yaml
name: Development Testing CI

on:
    pull_request:
        branches: ["development"]

jobs:
    test:
        uses: rhaversen/.github/.github/workflows/development-backend.yml@main
        with:
            node_version: "lts/*"
            run_unit_tests: true
            run_integration_tests: true
            run_development_tests: true
```

**Features:**
- Build with artifact caching
- Unit, integration, and development environment tests
- Linting and spellcheck
- Docker image build validation

### Development Testing - Frontend (`development-frontend.yml`)

PR testing pipeline for frontend Next.js projects.

**Usage:**
```yaml
name: Development Testing CI

on:
    pull_request:
        branches: ["development"]

jobs:
    test:
        uses: rhaversen/.github/.github/workflows/development-frontend.yml@main
        with:
            node_version: "lts/*"
            run_tests: true
```

**Features:**
- Build and Docker image validation
- Jest tests (optional)
- Linting and spellcheck

## Projects Using These Workflows

- ExsysBackend / ExsysFrontend
- GaslightBackend / GaslightCodeRunner / GaslightFrontend
- GroupSchedulerBackend / GroupSchedulerFrontend
- LifeTrackerBackend / LifeTrackerFrontend