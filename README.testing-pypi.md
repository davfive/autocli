# Testing TestPyPI Releases

After deploying to TestPyPI, validate the package before approving production deployment.

## Installation

Install from TestPyPI:

```bash
pip install autocli --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple/
```

The `--extra-index-url` allows pip to find dependencies from the main PyPI index.

## Validation

Check the installed version:

```bash
pip show autocli
```

Test that autocli can be imported:

```bash
python -c "from autocli import create_command_parser; print('OK')"
```

Try running one of the example applications:

```bash
cd examples/dunder_app
python run.py --help
```

## Approval

If validation passes, approve the `DeployGate_PyPiProd` gate in GitHub Actions to deploy to PyPI.