# Deployment Workflow

The `.github/workflows/deploy.yml` workflow handles package releases. It's triggered when you push a tag starting with `v` (e.g., `v0.1.0`).

The workflow uses two manual approval gates to prevent accidental releases to PyPI.

## Setup Required

The manual approval gates only work if you configure GitHub Environments in your repository settings. Without this setup, deployments run automatically without approval.

## Pipeline Stages

The workflow runs four sequential jobs:

1. **build** - Builds wheel and source distributions using `python -m build`
2. **deploy_github_packages** - Publishes to GitHub Packages (automatic backup)
3. **deploy_pypi_test** - Deploys to TestPyPI (requires approval at `DeployGate_PyPiTest`)
4. **deploy_pypi_prod** - Deploys to PyPI (requires approval at `DeployGate_PyPiProd`)

## Deployment Process

1. Tag a release: `git tag v0.1.0 && git push --tags`
2. Workflow builds package and deploys to GitHub Packages
3. Approve `DeployGate_PyPiTest` to deploy to TestPyPI
4. Test the TestPyPI release:
   ```bash
   pip install autocli --index-url https://test.pypi.org/simple/
   ```
5. Approve `DeployGate_PyPiProd` to deploy to PyPI

## Configuring Approval Gates

To enable manual approvals, configure GitHub Environments in your repository:

1. Go to repository Settings → Environments
2. Create environment `DeployGate_PyPiTest`:
   - Enable "Required reviewers"
   - Add approvers
3. Create environment `DeployGate_PyPiProd`:
   - Enable "Required reviewers"
   - Add approvers

Without this configuration, deployments run automatically without approval.
