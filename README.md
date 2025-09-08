# Complete Python Development Setup Guide (2025)

## Step 1: Install Core Tools

### Install UV (Python Package Manager)

```bash
# Windows (PowerShell as Administrator)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Restart your terminal, then verify
uv --version
```

### Install Git (if needed)

- Download: https://git-scm.com/downloads

```bash
# Restart your terminal, then verify
git --version
```

## Step 2: VSCode Setup

### Install Required Extensions

In VSCode install the following extensions:

1. **Python**
2. **Ruff**
3. **Even Better TOML**

## Step 3: Create Project Structure

### Initialize New Project

```bash
# Create and navigate to project
mkdir project-name
cd project-name

# Initialize with UV
uv init --python 3.11.6
```

`uv` will create the following files:

```
project-name/
├── .gitignore
├── .python-version
├── README.md
├── main.py
└── pyproject.toml
```

## Step 4: Configure Dependencies

### Install All Required Packages

If you already have the required dependencies in your `pyproject.toml`

```toml
dependencies = ["pandas>=2.3.2"]

[dependency-groups]
dev = [
    "ipykernel>=6.13.0",
    "pytest>=7.1.2",
    "pytest-cov>=3.0.0",
    "pre-commit==2.21.0",
    "mypy==1.6.0",
    "ruff>=0.5.3",
]
```

when you use `uv run` your environment will be crated automatically

```bash
uv run main.py
```

Otherwise, you can first create the env, and then install the dependencies manually.

```bash
# Create the environment
uv venv <your_env_name>

# Core dependencies for productioin
uv add "pandas>=2.3.2"

# Development dependencies with strict typing
uv add --dev "ipykernel>=6.13.0" "pytest>=7.1.2" "pytest-cov>=3.0.0" "pre-commit==2.21.0" "mypy==1.6.0" "ruff>=0.5.3"
```

## Step 5: Configure Checks

Add the required checks to the `pyproject.toml`

```toml
[tool.ruff]
line-length = 88
indent-width = 4

# Docstring formatting
[tool.ruff.lint.pydocstyle]
convention = "google"

[tool.ruff.lint]
extend-select = [
    "A",     # Flake8-builtins – misuse of Python built-in names
    "ANN",   # Flake8-annotations – enforce full type annotations everywhere
    "ARG",   # Flake8-unused-arguments – flags unused function arguments
    "ASYNC", # Flake8-async – checks async/await usage
    "B",     # Flake8-bugbear – common bug patterns
    "BLE",   # Flake8-blind-except – flags bare excepts
    "C4",    # Flake8-comprehensions – best practices in comprehensions
    "C90",   # McCabe – cyclomatic complexity metric
    "COM",   # Flake8-commas – trailing/comma issues
    "D",     # Pydocstyle – docstring formatting
    "DJ",    # Flake8-django – Django-specific conventions
    "DTZ",   # Flake8-datetimez – requires timezone-aware datetime objects
    "E",     # Pycodestyle errors (style issues)
    "EM",    # Flake8-errmsg – error message style
    "ERA",   # Eradicate – detects commented-out code
    "EXE",   # Flake8-executable – executable file checks
    "FA",    # Flake8-future-annotations – ensures future annotations
    "FBT",   # Flake8-boolean-trap – potential pitfalls with booleans
    "FIX",   # Flake8-fixme – flags FIXME comments
    "FLY",   # Flynt – f-string conversion suggestions
    "FURB",  # Refurb – code refurbishment suggestions
    "F401",  # Flake8 - Module imported but unused
    "F403",  # Flake8 - from module import * used
    "F841",  # Flake8 - Unused variables
    "G",     # Flake8-logging-format – logging format string issues
    "I",     # isort – import ordering checks
    "ICN",   # Flake8-import-conventions – enforces conventional import aliases
    "INP",   # Flake8-no-pep420 – warns against non-PEP420 namespace usage
    "INT",   # Flake8-gettext – checks for i18n usage
    "ISC",   # Flake8-implicit-str-concat – warns on implicit string concatenation
    "LOG",   # Flake8-logging – proper logging usage
    "N",     # PEP8-naming – naming conventions
    "NPY",   # NumPy-specific rules – ensures NumPy coding standards
    "PD",    # Pandas-vet – pandas-specific code practices
    "PERF",  # Perflint – performance-related checks
    "PGH",   # Pygrep-hooks – custom grep hooks
    "PIE",   # Flake8-pie – Python improvement suggestions
    "PL",    # Pylint – integration with Pylint conventions
    "PT",    # Flake8-pytest-style – pytest best practices
    "PTH",   # Flake8-use-pathlib – encourages pathlib over os.path
    "PYI",   # Flake8-pyi – type stub consistency
    "Q",     # Flake8-quotes – enforces quote style consistency
    "RET",   # Flake8-return – return statement issues
    "RSE",   # Flake8-raise – proper raise statements
    "RUF",   # Ruff-specific rules
    "S",     # Flake8-bandit – security issues
    "SIM",   # Flake8-simplify – code simplification hints
    "SLF",   # Flake8-self – instance methods that don’t use self
    "SLOT",  # Flake8-slots – suggests use of __slots__
    "T10",   # Flake8-debugger – debugger statements
    "TD",    # Flake8-todos – flags TODO comments
    "TID",   # Flake8-tidy-imports – import style enforcement
    "TRY",   # Tryceratops – try/except usage suggestions
    "UP",    # Pyupgrade – upgrades syntax to newer Python versions
    "W",     # Pycodestyle warnings (style issues)
    "YTT",   # Flake8-2020 – Python 2020 best practices

    # Opinionated / noisy ones (commented out for now):
    # "CPY",  # Flake8-copyright – requires copyright headers
    # "DOC",  # Pydoclint – hyper-strict docstring content enforcement
    # "T20",  # Flake8-print – disallows all print statements
]

extend-ignore = [
    "D100", # Ignore missing docstring in public module
]

[tool.quotes]
string = "single"
docstring = "triple-double"

[tool.lint.mypy]
ignore_missing_imports = true        # External packages without type stubs won’t block you
strict = true                        # Equivalent to enabling a bunch of safety flags below

# Useful additions even with `strict = true`
warn_return_any = true               # Flag functions that return `Any` — keeps type safety intact
warn_unused_configs = true           # Catch typos in this config (super handy)
warn_unreachable = true              # Warn if code is statically unreachable (dead code detection)

# Performance and clarity
show_error_codes = true              # Show error codes (like [arg-type]) so you can selectively silence/ignore
pretty = true                        # Make output more human-friendly
show_column_numbers = true           # Pinpoint error locations more precisely
show_error_context = true            # Print more context around errors (function signature, etc.)

# Optional stricter typing controls
disallow_untyped_defs = true         # Prevent defining functions without type hints
disallow_incomplete_defs = true      # Force full signatures, no partially typed functions
disallow_untyped_calls = true        # Block calling untyped functions from typed code
disallow_subclassing_any = true      # Don’t allow subclassing from `Any` (too loose)

# Null-handling
no_implicit_optional = true          # Require explicit `Optional[...]` instead of silently allowing None
strict_optional = true               # Enforce strict handling of `Optional[...]` values (already on with `strict`)

# Incremental build behavior
cache_fine_grained = true            # Speeds up repeated runs with fine-grained caching

```

## Step 6: Configure VSCode to Format Code on Save

Create the below file `.vscode/settings.json` and

```json
{
  "python.analysis.inlayHints.variableTypes": true,
  "python.analysis.inlayHints.functionReturnTypes": true,
  "python.analysis.typeCheckingMode": "off",
  "python.analysis.autoFormatStrings": true,
  "[python]": {
    "editor.defaultFormatter": "charliermarsh.ruff",
    "editor.formatOnSave": true,
    "editor.codeActionsOnSave": {
      "source.organizeImports": "explicit"
    }
  }
}
```

## Step 7: Pre-commit with Strict Type Checking

Add the right hooks to enable the stage differentiation:

```bash
uv run pre-commit install --hook-type pre-commit --hook-type commit-msg
```

Create a `.pre-commit-config.yaml`file.

```yaml
repos:
  # Ruff: linting & formatting
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.9.4
    hooks:
      - id: ruff
        stages: [commit]
      - id: ruff-format
        stages: [commit]

  # Mypy: type checking
  - repo: https://github.com/pre-commit/mirrors-mypy
    rev: v1.11.0
    hooks:
      - id: mypy
        stages: [commit]

  # Gitlint: commit message checks
  - repo: https://github.com/jorisroovers/gitlint
    rev: v0.19.1
    hooks:
      - id: gitlint
        stages: [commit-msg]
        pass_filenames: false
        args: ["--config=.gitlint"] # pass a custom config file
```

Add a `.gitlint` file in the root

```toml
[general]
ignore-merge-commits = true

[title-max-length]
line-length=50

[body-max-line-length]
line-length=72

[title-match-regex]
regex=^[A-Z]   # Require capitalized title
```

## Step 8: Enjoy the check

Now, comitting the code will trigger the checks on:

- the STAGED files
- the commit title and message
