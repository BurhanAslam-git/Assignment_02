# GitHub Actions Workflows

## CI (`.github/workflows/ci.yml`)
- Triggers on push/PR to `main`.
- Steps: checkout, setup Node 18, install, lint (if script exists), test (if script exists), build (if script exists).
- Uploads artifacts (coverage/junit if present).

## Deploy (`.github/workflows/deploy.yml`)
- Triggers on push to `main` or manual dispatch.
- Job 1 builds and uploads artifact; Job 2 deploys using provider-specific commands.
- Replace the placeholder deploy step with your platform (Heroku/AWS/Azure) and add required secrets.

## Scheduled Maintenance (`.github/workflows/scheduled-tasks.yml`)
- Runs daily at 03:00 UTC (and manual dispatch).
- Executes maintenance commands and uploads `maintenance.log`.

## Dependency Updates
- Dependabot config at `.github/dependabot.yml` checks npm weekly and avoids auto major bumps.
- Workflow `.github/workflows/dependency-updates.yml` tests Dependabot PRs and auto-merges minor/patch if tests pass.

## Code Review (`.github/workflows/code-review.yml`)
- On PRs to `main`, runs ESLint (if available) and CodeQL security analysis (JavaScript).
- Enforce checks via branch protection rules.

## Documentation (`.github/workflows/documentation.yml`)
- Builds docs when `docs/**` or `README.md` changes and deploys to GitHub Pages.
- Uses `actions/upload-pages-artifact` and `actions/deploy-pages`.
- Provide a `docs:build` script for custom static docs, otherwise a simple site is published from `docs/`.

## Custom Release Notes (`.github/workflows/custom-release-notes.yml`)
- On tag push `v*.*.*`, generates release notes from recent commits and publishes a GitHub Release.

### Secrets to configure
- `HEROKU_API_KEY`, `HEROKU_APP_NAME` for Heroku (if used).
- For AWS/Azure, add respective credentials and provider actions.

### Interpreting results
- Each workflow shows pass/fail per step; artifacts/logs are attached to runs.
- Pages deployment exposes a URL in the run summary/environment.
