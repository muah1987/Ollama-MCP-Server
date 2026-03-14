# Changelogs

This folder contains changelogs maintained by AI agents and contributors.

## Format

Each changelog file follows the naming convention: `YYYY-MM-DD.md`

Entries are appended in reverse chronological order within each file. Each entry includes:
- **Timestamp** (ISO 8601)
- **Agent/AI identifier** (`{agentuuid}` or `{aiuuid}`)
- **Summary of changes**
- **Files modified**

## Usage by AI Agents

AI agents **MUST** check this folder at the start of every session and append their
changes before completing their work. This ensures continuity across sessions and
prevents repeated mistakes.

See `.github/copilot-instructions.md` for detailed instructions.
