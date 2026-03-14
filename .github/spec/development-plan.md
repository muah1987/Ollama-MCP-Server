# Development Plan

**Last Updated:** 2026-03-14

## Current State

The Ollama MCP Server has been migrated from Node.js to Python 3.12+. The Python
implementation is feature-complete with 28 tools across 6 categories (Ollama, GitHub,
Git, Playwright, Infrastructure, MCP Gateway).

## Priorities

### High Priority

- [ ] Implement HTTP/SSE transport for `mcp_server.py` (currently only stdio is supported)
- [ ] Expand Python test coverage beyond basic functionality tests
- [ ] Fix Docker build pipeline (npm install timeout in container)

### Medium Priority

- [ ] Add integration tests for individual tool categories
- [ ] Add end-to-end tests with mock Ollama server
- [ ] Improve error handling and user-facing error messages
- [ ] Add request/response validation middleware

### Low Priority

- [ ] Archive or remove legacy Node.js code in `/legacy/`
- [ ] Add OpenAPI/AsyncAPI documentation generation
- [ ] Add metrics and observability (Prometheus, OpenTelemetry)
- [ ] Add rate limiting and request throttling

## Architecture Notes

### Tool Categories (28 tools total)
- **Ollama** (5 tools) — Model management, chat, generation
- **GitHub** (7 tools) — Repository, issue, PR management
- **Git** (4 tools) — Local git operations
- **Playwright** (9 tools) — Browser automation
- **Infrastructure** (3 tools) — Docker, Kubernetes
- **MCP Gateway** (6 tools) — Multi-server orchestration

### Key Design Decisions
- Pydantic for all configuration and validation
- structlog for structured logging
- Click for CLI interface
- aiohttp for async HTTP operations
- Abstract base class pattern for tool implementations
