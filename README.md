# OMCC: Oh My Claude Code

A single source of truth for Claude Code workflows. Tell Claude to set itself up and start building.

## One-Line Setup

In any project, tell Claude:

> **"Set up OMCC from https://github.com/SebastiaanWouters/omcc"**

Claude will automatically:
1. Fetch all skills, commands, and rules
2. Create the `.claude/` directory structure
3. Initialize beads tracking
4. Ask for your project details
5. Be ready to work

## What You Get

**3-Phase Auto-Detecting Workflow:**
1. **Planning** → Research, brainstorm, create `.plans/<feature>.md`
2. **Decomposition** → Break plans into beads (tasks) with dependencies
3. **Implementation** → Execute beads with validation and coordination

**6 Slash Commands:**
| Command | Purpose |
|---------|---------|
| `/plan <name>` | Start planning phase |
| `/decompose` | Break plan into beads |
| `/implement` | Start implementation |
| `/prime` | Initialize session |
| `/next-bead` | Find next task |
| `/status` | Show workflow state |

**Auto-Detection:** Just describe what you want to build. No manual mode switching.

## Prerequisites

Install the 5 required tools first:

```bash
# Beads (bd + bv)
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/beads/main/install.sh | bash

# CASS
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/coding_agent_session_search/main/install.sh | bash

# cass-memory (cm) - adjust for your OS
curl -L https://github.com/Dicklesworthstone/cass_memory_system/releases/latest/download/cm-linux-amd64 -o ~/.local/bin/cm && chmod +x ~/.local/bin/cm

# UBS
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/ultimate_bug_scanner/master/install.sh | bash

# Verify
bd --version && bv --version && cass --version && cm --version && ubs --version
```

## How It Works

When Claude sets up OMCC, it creates:

```
your-project/
├── .claude/
│   ├── skills/
│   │   ├── planning/          # Auto-detects planning needs
│   │   ├── decomposition/     # Auto-detects task breakdown
│   │   └── implementation/    # Auto-detects coding work
│   ├── commands/              # 6 slash commands
│   └── rules/                 # Safety constraints
├── .plans/                    # Planning outputs
├── .beads/                    # Task tracking
├── CLAUDE.md                  # Project context + workflow
└── CODEMAPS.md                # Architecture docs
```

## Usage

After setup, just describe what you want to build:

> "I need a user authentication system with JWT tokens"

Claude will:
- Detect you need planning
- Research your codebase
- Brainstorm approaches
- Create a structured plan
- Offer to break it into tasks
- Execute with validation

Or use explicit commands: `/plan auth-system`

## Optional: Multi-Agent Mode

For parallel work, set up Agent Mail MCP. See [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md).

## More Info

- [SETUP.md](SETUP.md) - Manual setup & troubleshooting
- [INIT.md](INIT.md) - What Claude does during setup
- [docs/TOOLS.md](docs/TOOLS.md) - Tool reference
- [docs/PHILOSOPHY.md](docs/PHILOSOPHY.md) - Why this workflow
- [examples/](examples/) - Sample plans and beads
