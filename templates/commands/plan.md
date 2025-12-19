---
description: Start the planning phase for a new feature or project
allowed-tools: Bash(cass:*), Bash(cm:*), Bash(mkdir:*), Bash(ls:*), Read, Write, Grep, Glob, WebSearch, Task, AskUserQuestion
argument-hint: <feature-name>
---

# Start Planning Phase

You are explicitly entering the **Planning Phase** for: $ARGUMENTS

## Setup

```bash
mkdir -p .plans
```

## Invoke Planning Skill

The planning skill contains the full workflow. Execute it now with this context:
- Feature/project name: $ARGUMENTS
- User has explicitly requested planning
- Skip auto-detection, go directly to context gathering

## Expected Output

Create: `.plans/$1.md` with structured plan content.

When complete, ask: "Plan ready. Want to decompose into tasks?"
