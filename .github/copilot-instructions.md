# Ollama MCP Server Development Guide

**ALWAYS follow these instructions first and only fallback to additional search and context gathering if the information here is incomplete or found to be in error.**

This is a **Python 3.12+** Model Context Protocol (MCP) server that provides Ollama AI model integration and DevOps tools. The server uses Pydantic, Click, aiohttp, and structlog.

> **Note:** Legacy Node.js code is archived in `/legacy/`. The active codebase is Python in `src/ollama_mcp_server/`.

## Quick Start & Critical Setup

**CRITICAL:** Set appropriate timeouts (120+ seconds) for all build and test commands. NEVER CANCEL builds or tests that are running.

### Bootstrap the Development Environment
```bash
# Clone and setup (if not already done)
git clone https://github.com/muah1987/Ollama-MCP-Server.git
cd Ollama-MCP-Server

# Install dependencies
pip install -e ".[dev]"
# or
pip install -r requirements.txt

# Validate setup
make lint             # Code quality check (flake8)
make test             # Run Python test suite (pytest)
```

## Working Development Commands

### Code Quality
```bash
# Linting with flake8
make lint             # Check for code quality issues
flake8 src/ tests/    # Direct flake8 check

# Formatting with black
black src/ tests/     # Auto-format code

# Type checking with mypy
mypy src/             # Static type analysis
```

### Testing
```bash
# Python test suite
make test                                    # Run all tests
pytest tests/python/                         # Run Python tests directly
pytest tests/python/ -v                      # Verbose output
pytest tests/python/ --cov=src/              # With coverage
pytest tests/python/ -k "test_name"          # Run specific test
```

### Combined Validation
```bash
make lint && make test    # Lint then test
```

## Agent Self-Improvement System

### Changelogs (`.github/changelogs/`)
AI agents **MUST** maintain changelogs to track all modifications:

1. **At session start:** Read the latest changelog in `.github/changelogs/` to understand recent changes
2. **Before completing work:** Append a new entry to the current date's changelog file (e.g., `2026-03-14.md`)
3. **Entry format:**
   ```markdown
   ## [ISO-8601-TIMESTAMP] {agent:AGENT_UUID}

   ### Summary
   Brief description of what was done.

   ### Changes
   - **Fixed** description of fix
   - **Added** description of addition
   - **Changed** description of change

   ### Files Modified
   - `path/to/file.py`
   ```

### Memory (`.github/memory/`)
AI agents **MUST** use the memory system to learn from past sessions:

1. **At session start:** Read `.github/memory/agent-log.md` to load context from previous sessions
2. **Before completing work:** Append a new session entry with:
   - **Timestamp** (ISO 8601)
   - **Agent/AI identifier** using `{agent:UUID}` or `{ai:UUID}` format
   - **Context** — what was the task, what was the state of the code
   - **Decisions** — what choices were made and why
   - **Known issues** — problems discovered or unresolved
   - **Lessons learned** — what to do differently next time

### Specifications (`.github/spec/`)
- **code-audit.md** — Latest code quality audit results
- **development-plan.md** — Roadmap and priorities

### Build Exclusion
The `.github/changelogs/`, `.github/memory/`, and `.github/spec/` folders are tracked
in git for agent continuity but excluded from build artifacts via `.gitignore` rules
for `artifacts/`, `dist/`, and `build/` directories.

## Repository Structure & Navigation

### Key Directories
```
Ollama-MCP-Server/
├── src/ollama_mcp_server/     # Main Python source code
│   ├── config/                # Configuration management (Pydantic)
│   │   └── settings.py        # DevOpsConfig, OllamaConfig
│   ├── server/                # MCP server implementation
│   │   └── mcp_server.py      # MCPDevOpsServer class
│   ├── tools/                 # MCP tool implementations (28 tools)
│   │   ├── base_tool.py       # Abstract base class
│   │   ├── ollama.py          # Ollama model tools (5)
│   │   ├── github.py          # GitHub API tools (7)
│   │   ├── git.py             # Git operations tools (4)
│   │   ├── playwright.py      # Browser automation tools (9)
│   │   ├── infrastructure.py  # Docker/K8s tools (3)
│   │   ├── mcp_gateway.py     # MCP gateway tools (6)
│   │   └── registry.py        # Tool registry
│   ├── utils/                 # Utility functions
│   │   └── logging.py         # Structured logging (structlog)
│   └── main.py                # CLI entry point (Click)
├── tests/
│   ├── python/                # Python test suite (pytest)
│   └── legacy/                # Archived Node.js tests
├── legacy/                    # Archived Node.js implementation
├── config/                    # Configuration files
├── docs/                      # Documentation
├── .github/
│   ├── copilot-instructions.md  # This file
│   ├── spec/                    # Specifications and plans
│   ├── changelogs/              # Agent changelog tracking
│   ├── memory/                  # Agent session memory
│   └── workflows/               # CI/CD pipelines
├── pyproject.toml             # Python project config
├── requirements.txt           # Python dependencies
├── Makefile                   # Development shortcuts
└── README.md                  # Main documentation
```

### Important Files to Check After Changes
- **src/ollama_mcp_server/config/settings.py**: After modifying configuration
- **src/ollama_mcp_server/tools/*.py**: After adding/modifying tools
- **src/ollama_mcp_server/tools/registry.py**: After adding new tool categories
- **src/ollama_mcp_server/server/mcp_server.py**: After changing server behavior
- **tests/python/**: Always run relevant tests after code changes

## Known Issues and Current Limitations

### ⚠️ HTTP Transport Not Implemented
```bash
# The --transport=http flag is available in the CLI but not yet implemented
# Using it will raise NotImplementedError with a descriptive message
# Always use --transport=stdio (the default)
```

### ⚠️ Legacy Node.js Tests
The legacy test suite in `tests/legacy/` has import issues with the MCP SDK:
- 5/7 suites pass, 2/7 fail due to SDK import errors
- These are legacy tests from the Node.js implementation
- Focus on Python tests in `tests/python/`

## Environment Configuration

### Key Environment Variables
```bash
OLLAMA_API_URL=http://localhost:11434    # Ollama API endpoint
GITHUB_TOKEN=ghp_...                    # GitHub API token
DEBUG=false                             # Debug logging
LOG_LEVEL=INFO                          # Log level
ENVIRONMENT=development                 # Environment name
TRANSPORT_TYPE=stdio                    # Transport type (stdio only)
```

## Development Workflow Best Practices

### Making Code Changes
1. **Read agent memory**: Check `.github/memory/agent-log.md`
2. **Read changelogs**: Check `.github/changelogs/` for recent changes
3. **Run linting**: `make lint` or `flake8 src/ tests/`
4. **Make minimal changes**: Focus on specific functionality
5. **Test immediately**: `pytest tests/python/` after each change
6. **Update changelog**: Append to `.github/changelogs/YYYY-MM-DD.md`
7. **Update memory**: Append to `.github/memory/agent-log.md`

### Adding New Tools
1. **Create tool class** extending `BaseTool` in `src/ollama_mcp_server/tools/`
2. **Register tool** in the tool registry
3. **Add tests** in `tests/python/`
4. **Update docs** if needed
5. **Run validation**: `make lint && make test`

### Debugging Issues
1. **Check agent memory first**: `.github/memory/agent-log.md`
2. **Check linting**: `make lint`
3. **Run specific tests**: `pytest tests/python/ -k "pattern"`
4. **Enable debug logging**: Set `DEBUG=true` and `LOG_LEVEL=DEBUG`
5. **Check configuration**: Review environment variables

## Troubleshooting Common Issues

### Import errors in legacy tests
- **Issue**: MCP SDK missing built files in legacy Node.js tests
- **Impact**: Only affects `tests/legacy/`, not the active Python codebase
- **Resolution**: Focus on Python tests in `tests/python/`

### Docker build failures
- **Issue**: Container builds may fail during dependency installation
- **Workaround**: Use local development environment
- **Status**: Known issue

**Remember**: This codebase is a Python project. Always check `.github/memory/agent-log.md` and `.github/changelogs/` before starting work to learn from previous sessions and avoid repeating mistakes.