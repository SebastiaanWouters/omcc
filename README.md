# OMCC: Oh My Claude Code

A single source of truth for Claude Code workflows. Reference this repo from your projects to get an auto-detecting, multi-phase development workflow.

## What It Does

OMCC provides a 3-phase workflow that Claude automatically detects and guides you through:

1. **Planning** → Research, brainstorm, create `.plans/<feature>.md`
2. **Decomposition** → Break plans into beads (tasks) with dependencies
3. **Implementation** → Execute beads with validation and coordination

No manual mode switching. The agent detects context and activates the right workflow.

## Quick Start

### 1. Install Tools

```bash
# All 5 tools are required
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/beads/main/install.sh | bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/coding_agent_session_search/main/install.sh | bash
curl -L https://github.com/Dicklesworthstone/cass_memory_system/releases/latest/download/cm-linux-amd64 -o ~/.local/bin/cm && chmod +x ~/.local/bin/cm
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/ultimate_bug_scanner/master/install.sh | bash

# Verify
bd --version && bv --version && cass --version && cm --version && ubs --version
```

### 2. Set Up Your Project

```bash
# Option A: Copy templates
cp -r /path/to/omcc/templates/* your-project/

# Option B: Reference OMCC (in your CLAUDE.md)
echo "See workflow: @/path/to/omcc/templates/project-agents.md" >> .claude/CLAUDE.md
```

### 3. Initialize Project

```bash
cd your-project
mkdir -p .claude/skills .claude/commands .claude/rules
bd init
git add .beads/ && git commit -m "Initialize OMCC workflow"
```

### 4. Start Working

Just describe what you want to build. The agent will:
- Detect you need planning
- Guide you through research
- Create a plan for approval
- Offer to decompose into tasks
- Execute with validation

Or use explicit commands:
- `/plan <feature>` - Start planning
- `/decompose` - Create beads from plan
- `/implement` - Start coding

## Workflow Overview

```
┌─────────────────────────────────────────────────────────┐
│                    PLANNING                             │
│  User describes feature → Agent researches →           │
│  Brainstorms approaches → Creates .plans/<name>.md     │
└────────────────────────────┬────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────┐
│                  DECOMPOSITION                          │
│  Loads plan → Creates beads interactively →            │
│  Maps dependencies → Stores in .beads/                 │
└────────────────────────────┬────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────┐
│                  IMPLEMENTATION                         │
│  Selects task (AI-prioritized) → Claims bead →         │
│  Implements → Validates (ubs) → Closes → Repeats       │
└─────────────────────────────────────────────────────────┘
```

## Multi-Agent Support

For parallel work, set up Agent Mail MCP. Agents will:
- Register and coordinate
- Reserve files before editing
- Announce claims and completions
- Prevent conflicts automatically

## File Structure

```
your-project/
├── .claude/
│   ├── CLAUDE.md          # Project context
│   ├── skills/            # Auto-detected workflows
│   ├── commands/          # /slash commands
│   └── rules/             # Safety constraints
├── .plans/                # Planning outputs
├── .beads/                # Task tracking
├── AGENTS.md              # Workflow instructions
└── CODEMAPS.md            # Architecture docs
```

## Commands Reference

| Command | Purpose |
|---------|---------|
| `/plan <name>` | Start planning phase |
| `/decompose` | Break plan into beads |
| `/implement` | Start implementation |
| `/prime` | Initialize session |
| `/next-bead` | Find next task |
| `/status` | Show workflow state |

## Tools

5 tools are required: `bd`, `bv`, `cass`, `cm`, `ubs`

**Critical**: Always use `--json` or `--robot` flags. Bare `bv` and `cass` commands hang agents.

See [docs/TOOLS.md](docs/TOOLS.md) for complete reference.

## @ Reference Syntax

In CLAUDE.md and other config files, use `@path/to/file` to include content from another file:

```markdown
See workflow: @AGENTS.md
See architecture: @CODEMAPS.md
```

This tells Claude to read the referenced file for context.

## More Info

- [SETUP.md](SETUP.md) - Detailed installation
- [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md) - Why this workflow
- [docs/TOOLS.md](docs/TOOLS.md) - Tool reference
