---
name: omcc-setup
description: |
  Initialize OMCC workflow in current project. Activated when user says "set up OMCC",
  "initialize OMCC", or points to the OMCC repo URL asking for setup.
---

# OMCC Setup Skill

You are setting up the OMCC workflow in this project.

## Step 1: Verify Tools

```bash
echo "Checking required tools..."
command -v bd >/dev/null && echo "✓ bd" || echo "✗ bd MISSING"
command -v bv >/dev/null && echo "✓ bv" || echo "✗ bv MISSING"
command -v cass >/dev/null && echo "✓ cass" || echo "✗ cass MISSING"
command -v cm >/dev/null && echo "✓ cm" || echo "✗ cm MISSING"
command -v ubs >/dev/null && echo "✓ ubs" || echo "✗ ubs MISSING"
```

If any tool is missing, tell user to install from: https://github.com/SebastiaanWouters/omcc/blob/main/SETUP.md

## Step 2: Create Structure

```bash
mkdir -p .claude/skills/planning .claude/skills/decomposition .claude/skills/implementation
mkdir -p .claude/commands .claude/rules .plans
```

## Step 3: Fetch Templates

Use WebFetch to get each file and Write to create it:

**Skills:**
- Fetch `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/templates/skills/planning/SKILL.md`
- Write to `.claude/skills/planning/SKILL.md`
- Repeat for decomposition and implementation

**Commands:**
- Fetch and write all 6 command files from `templates/commands/`

**Rules:**
- Fetch and write both rule files from `templates/rules/`

**Project files:**
- Fetch `templates/project-agents.md` → `AGENTS.md`
- Fetch `templates/project-codemaps.md` → `CODEMAPS.md`

## Step 4: Create CLAUDE.md

Ask user for project details, then create `.claude/CLAUDE.md`:

```markdown
# Project: [NAME]

## Tech Stack
[FROM USER]

## Key Directories
[FROM USER]

## Commands
- Build: [FROM USER]
- Test: [FROM USER]
- Run: [FROM USER]

## Workflow
This project uses OMCC. See @AGENTS.md for workflow details.
```

## Step 5: Initialize Beads

```bash
bd init
cass index --full 2>/dev/null || true
```

## Step 6: Confirm

Tell user setup is complete and show available commands.
