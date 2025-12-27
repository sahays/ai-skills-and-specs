---
name: deployment-automation
description:
  Create robust deployment scripts for Docker containers on self-managed Rocky Linux servers. Use when writing
  deployment automation with full or code-only deployment modes. Focuses on fail-fast, prerequisite checking, and single
  environment file management.
---

# Deployment Automation

## Deployment Modes

**Full deployment**: Infrastructure setup + Docker containers + configuration.

**Code-only deployment**: Only Docker containers, assumes infrastructure exists.

**Single switch**: `--mode=full` or `--mode=code` controls behavior.

**See**: [references/script-structure.sh](references/script-structure.sh) for entry point with mode parsing.

## Script Structure

**Fail fast**: `set -euo pipefail` - Exit immediately on any error. Never continue with partial state.

**Functions over inline**: Each step as separate function. Clear, testable, reusable.

**Error handling**: Trap errors with line numbers. Log all failures.

**Exit codes**: 0 (success), 1 (validation failure), 2 (deployment failure), 3 (health check failure).

## Essential Checks

**Pre-deployment validation**:

- Prod env file exists with correct permissions (600)
- All required variables set in prod.env
- Docker is running
- Sufficient disk space (5GB minimum)

**Fail immediately if any check fails**: Don't deploy broken config.

**See**: [references/validation.sh](references/validation.sh) for complete validation functions.

## Dependency Management

**Check and install system dependencies** (Rocky Linux):

- docker-ce
- docker-compose-plugin
- git, curl, tar

**Only install in full mode**: Code-only mode fails if dependencies missing.

**See**: [references/dependencies.sh](references/dependencies.sh) for dependency checking and installation.

## Environment File Management

**Single source of truth**: `/app/config/prod.env`

**Requirements**:

- Must exist (never created by script)
- Owned by deploy user
- 600 permissions (owner read/write only)
- ENVIRONMENT must be "prod"
- All required variables present and non-empty

**See**: [references/validation.sh](references/validation.sh) for env file validation.

## Full Deployment Mode

**Infrastructure setup**:

1. Install dependencies
2. Create directory structure (/app/{config,data,logs,releases})
3. Set ownership (deploy:deploy)
4. Setup Docker network
5. Install systemd service
6. Continue with code deployment

**See**: [references/deployment.sh](references/deployment.sh) for full deployment function.

## Code-Only Deployment Mode

**Docker container deployment**:

1. Create timestamped release directory
2. Extract release artifact
3. Copy prod.env into release
4. Pull Docker images
5. Stop current containers gracefully
6. Update symlink atomically
7. Start new containers
8. Wait for health check
9. Cleanup old releases (keep last 3)

**Atomic symlink swap**: Current always points to working version.

**See**: [references/deployment.sh](references/deployment.sh) for code deployment function.

## Health Checks

**Wait for service to be healthy**:

- Poll health endpoint with retries (30 attempts, 2s interval)
- Automatic rollback if health check fails
- Never leave broken deployment running

**See**: [references/health-rollback.sh](references/health-rollback.sh) for health check implementation.

## Rollback

**Simple rollback to previous version**:

1. Stop broken version
2. Find previous release
3. Revert symlink
4. Start previous version

**Keep previous releases**: Don't delete until new version proven stable.

**See**: [references/health-rollback.sh](references/health-rollback.sh) for rollback function.

## Directory Structure

```
/app/
├── config/
│   └── prod.env          # Single prod environment file (600 perms)
├── current -> releases/20231215-143022/  # Symlink to active release
├── data/                 # Persistent data
├── logs/                 # Application logs
└── releases/
    ├── 20231215-143022/  # Previous release
    ├── 20231215-150033/  # Current release
    └── 20231215-152041/  # Failed release (will be cleaned)
```

## Docker Compose Integration

**Environment from prod.env**: Copied to each release as `.env`

**Health check in compose**: Docker monitors container health

**See**: [references/docker-compose.yml](references/docker-compose.yml) for example configuration.

## Systemd Service

**Manage containers with systemd**: Auto-start on boot, graceful shutdown

**See**: [references/systemd-service.sh](references/systemd-service.sh) for systemd service installation.

## Cleanup

**Remove old releases**: Keep last 3 releases for rollback

**Run after successful deployment**: Only cleanup after new version proven stable

**See**: [references/deployment.sh](references/deployment.sh) for cleanup function.

## Logging

**Log all deployment activities**:

- Deployment start/end with timestamp
- Each step executed
- Health check results
- Any errors with line numbers

**Daily log files**: `/app/logs/deploy-YYYYMMDD.log`

## Prerequisites

**Rocky Linux packages**: docker-ce, docker-compose-plugin, git, curl, tar

**Install Docker**: See [references/dependencies.sh](references/dependencies.sh) for installation script.

**Full mode installs missing prerequisites**: Code-only fails if not present.

## Common Pitfalls

**Don't**:

- Skip health checks
- Deploy without validating env file
- Delete previous version immediately
- Run as root (use deploy user)
- Commit prod.env to git
- Ignore failed containers

**Do**:

- Use --mode=full for first deployment
- Use --mode=code for subsequent deployments
- Validate env file before every deployment
- Check Docker logs if deployment fails
- Keep last 3 releases for rollback
- Test deployment script in dev first
