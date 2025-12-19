# OMCC Auto-Setup Instructions

When a user points you to this repo and asks you to "set yourself up" or "initialize OMCC", follow these steps:

## Step 1: Verify Tools

First, check if required tools are installed:

```bash
command -v bd >/dev/null && echo "✓ bd" || echo "✗ bd - INSTALL REQUIRED"
command -v bv >/dev/null && echo "✓ bv" || echo "✗ bv - INSTALL REQUIRED"
command -v cass >/dev/null && echo "✓ cass" || echo "✗ cass - INSTALL REQUIRED"
command -v cm >/dev/null && echo "✓ cm" || echo "✗ cm - INSTALL REQUIRED"
command -v ubs >/dev/null && echo "✓ ubs" || echo "✗ ubs - INSTALL REQUIRED"
```

If any are missing, show the user the install commands from SETUP.md and wait for them to install.

## Step 2: Create Directory Structure

```bash
mkdir -p .claude/skills/planning
mkdir -p .claude/skills/decomposition
mkdir -p .claude/skills/implementation
mkdir -p .claude/commands
mkdir -p .claude/rules
mkdir -p .plans
```

## Step 3: Fetch and Create Files

Fetch each file from the raw GitHub URL and create it locally:

### Skills (3 files)
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/skills/planning/SKILL.md` → `.claude/skills/planning/SKILL.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/skills/decomposition/SKILL.md` → `.claude/skills/decomposition/SKILL.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/skills/implementation/SKILL.md` → `.claude/skills/implementation/SKILL.md`

### Commands (6 files)
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/commands/plan.md` → `.claude/commands/plan.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/commands/decompose.md` → `.claude/commands/decompose.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/commands/implement.md` → `.claude/commands/implement.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/commands/prime.md` → `.claude/commands/prime.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/commands/next-bead.md` → `.claude/commands/next-bead.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/commands/status.md` → `.claude/commands/status.md`

### Rules (2 files)
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/rules/safety.md` → `.claude/rules/safety.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/rules/coordination.md` → `.claude/rules/coordination.md`

### Project Files (3 files)
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/project-claude.md` → `.claude/CLAUDE.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/project-agents.md` → `AGENTS.md`
- `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/project-codemaps.md` → `CODEMAPS.md`

## Step 4: Initialize Beads

```bash
bd init
```

## Step 5: Customize CLAUDE.md

Ask the user for:
- Project name
- Tech stack
- Key directories
- Build/test/run commands

Update `.claude/CLAUDE.md` with their answers.

## Step 6: Index Sessions

```bash
cass index --full 2>/dev/null || echo "No sessions to index yet"
```

## Step 7: Confirm Setup

Show the user:
```
✓ OMCC initialized successfully!

Created:
- .claude/skills/ (3 auto-detected workflows)
- .claude/commands/ (6 slash commands)
- .claude/rules/ (2 safety rules)
- .plans/ (for planning outputs)
- AGENTS.md (workflow reference)
- CODEMAPS.md (architecture docs)

Available commands:
- /plan <feature> - Start planning
- /decompose - Break plan into tasks
- /implement - Start coding
- /prime - Initialize session
- /next-bead - Find next task
- /status - Show current state

Ready to work! Describe what you want to build.
```
