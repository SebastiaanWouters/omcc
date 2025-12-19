# OMCC Auto-Setup Instructions

When a user points you to this repo and asks to "set up OMCC", follow these steps:

## Step 1: Verify Tools

```bash
echo "Checking tools..."
MISSING=""
command -v bd >/dev/null || MISSING="$MISSING bd"
command -v bv >/dev/null || MISSING="$MISSING bv"
command -v cass >/dev/null || MISSING="$MISSING cass"
command -v cm >/dev/null || MISSING="$MISSING cm"
command -v ubs >/dev/null || MISSING="$MISSING ubs"

if [ -n "$MISSING" ]; then
  echo "Missing:$MISSING"
else
  echo "All tools installed ✓"
fi
```

If missing, show install commands and wait for user.

## Step 2: Create Structure

```bash
mkdir -p .claude/skills/planning .claude/skills/decomposition .claude/skills/implementation
mkdir -p .claude/commands .claude/rules .plans
```

## Step 3: Fetch Templates

Use WebFetch to get each file from raw GitHub URLs, then Write locally:

| Source | Destination |
|--------|-------------|
| `templates/skills/planning/SKILL.md` | `.claude/skills/planning/SKILL.md` |
| `templates/skills/decomposition/SKILL.md` | `.claude/skills/decomposition/SKILL.md` |
| `templates/skills/implementation/SKILL.md` | `.claude/skills/implementation/SKILL.md` |
| `templates/commands/*.md` (6 files) | `.claude/commands/*.md` |
| `templates/rules/safety.md` | `.claude/rules/safety.md` |
| `templates/rules/coordination.md` | `.claude/rules/coordination.md` |
| `templates/project-codemaps.md` | `CODEMAPS.md` |

Base URL: `https://raw.githubusercontent.com/SebastiaanWouters/omcc/main/`

## Step 4: Smart Customization (IMPORTANT)

Before creating CLAUDE.md, gather context intelligently:

### Auto-Detect What You Can

```bash
# Detect project name
basename $(pwd)

# Detect package manager / language
ls package.json 2>/dev/null && echo "Node.js project"
ls Cargo.toml 2>/dev/null && echo "Rust project"
ls pyproject.toml requirements.txt 2>/dev/null && echo "Python project"
ls go.mod 2>/dev/null && echo "Go project"

# Detect existing structure
ls -la

# Check for existing scripts
cat package.json 2>/dev/null | grep -A 20 '"scripts"'
cat Makefile 2>/dev/null | head -30
```

### Ask Only What You Can't Detect

Use AskUserQuestion with smart defaults:

```
Questions to ask (skip if auto-detected):

1. "What does this project do?" (always ask - 1 sentence)

2. "Tech stack?" (only if not detected)
   Options based on detected files, plus "Other"

3. "Key directories to know about?"
   Show detected structure, ask if anything special

4. "Any patterns or conventions I should follow?"
   e.g., "Use functional components", "All errors go to Sentry"

5. "Any specific rules or guidelines for the AI?" (ALWAYS ASK)
   e.g., "No emojis", "Always write tests", "Use TypeScript strict mode"
   This creates custom rules - important for consistent AI behavior!
```

### Handle Custom AI Rules

If user provides AI guidelines:

1. **Create `.claude/rules/project.md`**:
```markdown
# Project-Specific Rules

{user_provided_rules - each as a bullet point}
```

2. **Add summary to CLAUDE.md** under "## AI Guidelines" section

### Create CLAUDE.md

Replace placeholders with gathered info:

```markdown
# {detected_name}

## Overview
{user_description}

## Tech Stack
{detected_or_asked}

## Structure
```
{detected_structure}
```

## Commands
```bash
{detected_from_package_json_or_makefile}
```

## Workflow
[OMCC workflow section - keep as-is from template]

## Patterns & Conventions
{user_provided_or_none}

## AI Guidelines
{user_rules_summary_or_"See .claude/rules/ for project rules"}

## Key Files
{detected_entry_points}
```

## Step 5: Initialize

```bash
bd init
cass index --full 2>/dev/null || true
```

## Step 6: Confirm

```
✓ OMCC initialized!

Created:
- .claude/ (skills, commands, rules)
- .plans/ (for planning outputs)
- CODEMAPS.md (architecture docs - fill in as you build)

Commands: /plan, /decompose, /implement, /status

What would you like to build?
```

---

## Key Principles

1. **Detect before asking** - Don't ask what you can figure out
2. **Smart defaults** - Offer detected values as options
3. **Minimal questions** - 2-4 questions max
4. **Useful output** - CLAUDE.md should actually help future sessions
5. **Fast setup** - Entire process < 2 minutes
