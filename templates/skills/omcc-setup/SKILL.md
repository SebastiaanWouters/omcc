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
- Fetch `templates/project-codemaps.md` → `CODEMAPS.md`
- Fetch `templates/project-claude.md` → use as template for `CLAUDE.md`

## Step 4: Create CLAUDE.md (Smart Customization)

Use smart auto-detection before asking questions:

1. **Auto-detect** project name, tech stack, structure from files
2. **Ask only** what can't be detected (project purpose, special conventions)
3. **ALWAYS ask**: "Any specific rules or guidelines for the AI to follow?"
4. **Fill template** from `templates/project-claude.md` with gathered info

If user provides AI rules:
- Create `.claude/rules/project.md` with their rules as bullet points
- Add summary to CLAUDE.md under "## AI Guidelines"

See INIT.md for detailed customization workflow.

## Step 5: Initialize Beads

```bash
bd init
cass index --full 2>/dev/null || true
```

## Step 6: Confirm

Tell user setup is complete and show available commands.
