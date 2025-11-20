# Installing from GitHub Packages

GitHub Packages provides access to development and pre-release versions.

## Setup

Authenticate with GitHub CLI:

```bash
brew install gh  # or your preferred installation method
gh auth login
```

Configure pip credentials:

```bash
chmod +x scripts/setup_pypirc.sh
./scripts/setup_pypirc.sh
```

The script configures `~/.pypirc` with your GitHub token. If you already have a `.pypirc`, it creates `~/.pypirc.autocli.rc` which you'll need to merge manually.

## Installation

```bash
pip install autocli --index-url https://pypi.pkg.github.com/davfive/simple/
```