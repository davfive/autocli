# Testing

The test suite validates autocli's command discovery and execution across different organizational patterns.

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

Run a specific test:
```bash
pytest tests/test_examples.py::TestDunderApp::test_user__delete
```

## Test Coverage

The tests use `subprocess` to execute the example applications end-to-end, validating:
- Command discovery from different directory structures
- Argument parsing through the full stack
- Correct output and exit codes

This approach ensures the entire autocli workflow functions correctly, not just individual units.

## Troubleshooting

**Import errors**: Run from project root with dependencies installed.

**Path issues**: Tests use `pathlib` to locate example directories relative to the test file location.