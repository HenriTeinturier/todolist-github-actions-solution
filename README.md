# CI/CD Pipeline TodoList App

## Workflow Overview

This CI/CD pipeline automates the testing, building, and deployment process of the TodoList application.

### Pipeline Steps

1. **Tests and Coverage** (`Test on Node 18` and `Test on Node lts/*`)

   - Runs unit tests
   - Generates coverage reports
   - Uploads coverage artifacts

2. **Lint** (`Lint code`)

   - Checks code style and quality

3. **Build** (`Build application`)

   - Builds the application
   - Depends on successful tests and lint

4. **Development Deployment** (`Deploy to development`)

   - Deploys to development environment
   - Triggers on feature/\* branches PRs to develop
   - Auto-merges PR if successful

5. **Production Deployment** (`Deploy to production`)
   - Deploys to production environment
   - Triggers when PRs are merged into main

### Branch Protection Rules

- `develop` branch requires:

  - Passing status checks
  - Pull request review
  - Up-to-date branch

- `main` branch requires:
  - Passing status checks
  - Pull request review
  - Up-to-date branch
  - Environment deployment

### How to Re-run Checks

1. Go to the Pull Request page
2. Click on "Conversation" tab
3. Find the "Checks" section (usually at the bottom of the PR description)
4. Click on the "Details" link next to any check
5. In the top right corner, click on the three dots (⋮)
6. Select "Re-run all jobs"

Alternatively, you can:

1. Go to the "Actions" tab in your repository
2. Find the workflow run you want to re-run
3. Click the "Re-run all jobs" button

### Environment Configuration

- **Development Environment**

  - Target branches: `develop`, `feature/**`
  - Required variables:
    - `DEPLOY_URL`: Development deployment URL
    - `DEPLOY_TOKEN`: Development deployment token

- **Production Environment**
  - Target branch: `main`
  - Required variables:
    - `DEPLOY_URL`: Production deployment URL
    - `DEPLOY_TOKEN`: Production deployment token
