# autocli

[![PyPI version](https://badge.fury.io/py/autocli.svg)](https://badge.fury.io/py/autocli)
[![Python Support](https://img.shields.io/pypi/pyversions/autocli.svg)](https://pypi.org/project/autocli/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**A dynamic argparse command loader for modular CLIs using `__` separation.**

autocli simplifies building complex command-line interfaces by automatically discovering and organizing subcommands from your Python modules. Define commands as simple Python files, and autocli handles the argparse boilerplate.

## Features

- 🚀 **Zero Boilerplate**: No manual subparser registration needed
- 📁 **Flexible Organization**: Support for both directory hierarchies and `__` filename separation
- 🔄 **Mixed Patterns**: Combine directories and `__` separators in the same project
- 🎯 **Simple API**: Just two required functions per command module
- 🧩 **Modular Design**: Easy to add, remove, or reorganize commands
- ⚡ **Automatic Discovery**: Commands are discovered recursively from your package

## Installation

Install autocli using pip:

```bash
pip install autocli
```

## Quick Start

### 1. Create Your Command Structure

You can organize commands using directories, `__` separators in filenames, or both:

**Directory-based** (`commands/user/add.py`):
```
commands/
├── __init__.py
└── user/
    ├── __init__.py
    └── add.py
```

**Filename-based** (`commands/user__add.py`):
```
commands/
├── __init__.py
└── user__add.py
```

**Mixed** (`commands/admin__db/connect.py`):
```
commands/
├── __init__.py
└── admin__db/
    ├── __init__.py
    └── connect.py
```

### 2. Define Your Command Module

Each command file needs two functions:

```python
# commands/user__add.py
import argparse

def autocli_setup_parser(subparsers, command_name):
    """Sets up the argument parser for this command."""
    parser = subparsers.add_parser(
        command_name,
        help="Add a new user"
    )
    parser.add_argument("username", help="Username for the new user")
    parser.add_argument("--email", help="User's email address")
    
    # Link the execution function
    parser.set_defaults(func=run_command)

def run_command(args):
    """Executed when the command is run."""
    print(f"Creating user: {args.username}")
    if args.email:
        print(f"Email: {args.email}")
```

### 3. Create Your CLI Entry Point

```python
# run.py
from autocli import create_command_parser
import commands

def main():
    parser = create_command_parser(
        commands,
        description="My awesome CLI application"
    )
    args = parser.parse_args()
    
    if hasattr(args, "func"):
        args.func(args)
    else:
        parser.print_help()

if __name__ == "__main__":
    main()
```

### 4. Run Your CLI

```bash
$ python run.py user add john --email john@example.com
Creating user: john
Email: john@example.com

$ python run.py user --help
usage: run.py user [-h] {add} ...

positional arguments:
  {add}
    add       Add a new user
```

## Command Organization Patterns

### Directory Hierarchy

Best for larger projects with many related commands:

```
commands/
├── user/
│   ├── add.py      → user add
│   └── delete.py   → user delete
└── admin/
    └── settings/
        └── config.py → admin settings config
```

### Dunder (`__`) Separation

Best for flatter structures or when you prefer single-file commands:

```
commands/
├── user__add.py      → user add
├── user__delete.py   → user delete
└── admin__config.py  → admin config
```

### Mixed Approach

Combine both patterns for maximum flexibility:

```
commands/
├── user__db/
│   ├── connect.py    → user db connect
│   └── backup.py     → user db backup
└── admin/
    └── user__list.py → admin user list
```

## Examples

The repository includes several complete examples demonstrating different organizational patterns:

- **[single_app](examples/single_app/)**: Simple single-level commands
- **[dunder_app](examples/dunder_app/)**: Using `__` filename separation
- **[dirs_app](examples/dirs_app/)**: Directory-based hierarchy
- **[mixed_app](examples/mixed_app/)**: Combining both approaches

Run any example:

```bash
cd examples/dunder_app
python run.py --help
```

## Requirements

- Python 3.8 or higher
- No external dependencies (uses only standard library)

## Documentation

For more detailed information, see:

- [Testing Guide](tests/README.md)
- [Deployment Workflow](README.deploy.md)
- [PyPI Testing](README.testing-pypi.md)
- [GitHub Testing](README.testing-github.md)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. For major changes, please open an issue first to discuss what you would like to change.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Author

**davfive** - [davfive@gmail.com](mailto:davfive@gmail.com)

## Links

- [PyPI Package](https://pypi.org/project/autocli/)
- [Source Code](https://github.com/davfive/autocli)
- [Issue Tracker](https://github.com/davfive/autocli/issues)