# {PROJECT_NAME}

## Overview
{DESCRIPTION}

## Tech Stack
{TECH_STACK}

## Structure
```
{DIRECTORY_STRUCTURE}
```

## Commands
```bash
# Build
{BUILD_CMD}

# Test
{TEST_CMD}

# Run
{RUN_CMD}

# Lint
{LINT_CMD}
```

## Workflow

This project uses OMCC (Planning → Decomposition → Implementation).

### Phase Detection
- Describing new features → Planning phase activates
- `.plans/` exists + "break down" → Decomposition activates
- `.beads/` has tasks + "implement" → Implementation activates

### Commands
| Command | Purpose |
|---------|---------|
| `/plan <feature>` | Start planning |
| `/decompose` | Create tasks from plan |
| `/implement` | Execute tasks |
| `/status` | Show current state |

### Tools (Required)
```bash
bd ready --json      # List available tasks
bv --robot-triage    # Get AI recommendations
cass search "x" --robot  # Search past sessions
cm context "x" --json    # Get learned patterns
ubs --staged         # Scan for bugs before commit
```

**Never run bare `bv` or `cass` - they hang!**

## Architecture

See CODEMAPS.md for detailed architecture (create as you build).

## Patterns & Conventions

{PATTERNS}

## AI Guidelines

{AI_RULES}

## Key Files

{KEY_FILES}
