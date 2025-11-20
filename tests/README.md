# Testing

The test suite validates autocli's command discovery and execution by running example applications as end-to-end integration tests.

## Quick Start

Install with testing dependencies and run:

```bash
pip install -e ".[testing]"
pytest
```

## Running Tests

Run all tests:
```bash
pytest
```

Run with verbose output:
```bash
pytest -v
```

Run a specific test file:
```bash
pytest tests/test_examples.py
```

Run a specific test class or method:
```bash
pytest tests/test_examples.py::TestDunderApp
pytest tests/test_examples.py::TestDunderApp::test_user__delete
```

## How Tests Work

Tests dynamically discover and execute example applications using `subprocess`. The test suite:
- Automatically scans example app directories for command modules
- Generates test methods for each discovered command
- Validates command execution, output, and exit codes

This approach tests the full autocli workflow without coupling tests to specific command implementations.

## Troubleshooting

**Import errors**: Run from project root with dependencies installed.

**Test failures**: Check that example apps in `examples/` directory have valid command module structure.