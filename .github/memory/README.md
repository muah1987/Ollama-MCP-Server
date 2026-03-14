# Agent Memory

This folder stores session memory for AI agents working on this repository.
Each entry is timestamped and tagged with the agent/AI identifier.

## Purpose

- **Continuity** — Agents load previous memory to avoid repeating mistakes
- **Learning** — Patterns and decisions are recorded for future reference
- **Accountability** — Every agent session is logged with its UUID

## Format

Each entry in `agent-log.md` contains:
- **Timestamp** (ISO 8601)
- **Agent ID** (`{agentuuid}` or `{aiuuid}`)
- **Session context** (what was done, what was learned)
- **Decisions made** (and why)
- **Known issues** (to avoid in future sessions)

## Usage by AI Agents

AI agents **MUST**:
1. Read `agent-log.md` at the start of every session
2. Append a new entry before completing their work
3. Include their agent/AI UUID in every entry
4. Document any mistakes made and how they were resolved

See `.github/copilot-instructions.md` for detailed instructions.
