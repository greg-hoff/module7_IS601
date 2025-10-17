# GitHub Actions Setup for QR Code Maker

This repository uses GitHub Actions for automated CI/CD pipeline that builds, tests, and deploys the Docker image.

## Required Secrets

To enable automatic Docker Hub publishing, you need to set up these secrets in your GitHub repository:

### Setting up Docker Hub Secrets

1. **Go to your GitHub repository**
2. **Click Settings → Secrets and variables → Actions**
3. **Add the following repository secrets:**

#### DOCKER_USERNAME
- **Name:** `DOCKER_USERNAME`
- **Value:** Your Docker Hub username (`greghoff`)

#### DOCKER_TOKEN
- **Name:** `DOCKER_TOKEN`
- **Value:** Your Docker Hub access token

**To create a Docker Hub access token:**
1. Log in to [Docker Hub](https://hub.docker.com/)
2. Click your profile → Account Settings
3. Go to Security → Access Tokens
4. Click "New Access Token"
5. Give it a name (e.g., "GitHub Actions")
6. Select permissions: Read, Write, Delete
7. Copy the generated token (you won't see it again!)
8. Use this token as the `DOCKER_TOKEN` secret value

## Workflows

### CI/CD Pipeline (`ci-cd.yml`)

**Triggers:**
- Push to `main` branch
- Pull requests to `main` branch

**Jobs:**
1. **Test Job** - Runs on all triggers
   - Sets up Python 3.12
   - Installs dependencies
   - Tests QR code generation functionality

2. **Build and Push Job** - Only on push to `main`
   - Builds multi-architecture Docker image (AMD64 and ARM64)
   - Pushes to Docker Hub with multiple tags:
     - `latest` (for main branch)
     - `main-<commit-sha>` (commit-specific)
     - Branch name tag
   - Tests the built Docker image

3. **Security Scan Job** - After successful build
   - Scans Docker image for vulnerabilities using Trivy
   - Uploads results to GitHub Security tab

## Workflow Features

- ✅ **Multi-architecture builds** (AMD64 and ARM64)
- ✅ **Build caching** for faster subsequent builds
- ✅ **Automated testing** of both Python code and Docker image
- ✅ **Security scanning** with vulnerability reporting
- ✅ **Smart tagging** with commit SHA and branch names
- ✅ **Only pushes on main branch** (not on PRs)

## Manual Workflow Triggers

You can also manually trigger workflows from the GitHub Actions tab in your repository.

## Monitoring

- View workflow runs in the **Actions** tab of your GitHub repository
- Security scan results appear in the **Security** tab
- Docker images are automatically published to Docker Hub after successful builds