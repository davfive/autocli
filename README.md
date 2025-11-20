# autocli

[![PyPI version](https://badge.fury.io/py/autocli.svg)](https://badge.fury.io/py/autocli)
[![Python Support](https://img.shields.io/pypi/pyversions/autocli.svg)](https://pypi.org/project/autocli/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A dynamic argparse command loader for building modular CLIs.

autocli automatically discovers and registers subcommands from your Python modules, eliminating the need for manual argparse subparser registration. Organize your commands using directory hierarchies, double-underscore (`__`) filename separation, or a mix of both.

## Why autocli?

If you're building a CLI with multiple subcommands, you've probably written code like this dozens of times:

```python
subparsers = parser.add_subparsers()
user_parser = subparsers.add_parser('user')
user_subparsers = user_parser.add_subparsers()
add_parser = user_subparsers.add_parser('add')
# ... and so on
```

autocli eliminates this boilerplate. Drop your command files into a directory structure, and they're automatically registered. Each command needs just two functions: one to configure its arguments, and one to execute.

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
    parser = create_command_parser(commands, description="My CLI tool")
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

## Command Organization

You have three options for organizing commands:

**Directory hierarchy** - Good for larger projects:
```
commands/
├── user/
│   ├── add.py      → user add
│   └── delete.py   → user delete
└── admin/
    └── settings/
        └── config.py → admin settings config
```

**Filename separation with `__`** - Good for flatter structures:
```
commands/
├── user__add.py      → user add
├── user__delete.py   → user delete
└── admin__config.py  → admin config
```

**Mixed approach** - Use both:
```
commands/
├── user__db/
│   ├── connect.py    → user db connect
│   └── backup.py     → user db backup
└── admin/
    └── user__list.py → admin user list
```

## Examples

The `examples/` directory contains working examples:

- `single_app/` - Simple single-level commands
- `dunder_app/` - Using `__` filename separation
- `dirs_app/` - Directory-based hierarchy
- `mixed_app/` - Combining both approaches

Try them out:
```bash
cd examples/dunder_app
python run.py --help
```

## Requirements

Python 3.8+ with no external dependencies.

## Development

Run tests:
```bash
pip install -e ".[testing]"
pytest
```

See [tests/README.md](tests/README.md) for details.

## Contributing

Pull requests welcome. For major changes, open an issue first.

## License

MIT License - see [LICENSE](LICENSE) for details.

Copyright (c) 2025 davfive ([davfive@gmail.com](mailto:davfive@gmail.com))