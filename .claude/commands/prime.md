---
description: Initialize agent session - read context, check state, find tasks
allowed-tools: Bash(bd:*), Bash(bv:*), Bash(cass:*), Bash(cm:*), Bash(git:*), Bash(cat:*), Bash(ls:*), Read
---

# Agent Startup Protocol

You are starting a new work session. Complete this checklist to orient yourself.

## 1. Announce Identity

```bash
echo "Agent session started: $(date)"
```

## 2. Verify Tools

```bash
command -v bd >/dev/null && echo "✓ bd" || echo "✗ bd MISSING"
command -v bv >/dev/null && echo "✓ bv" || echo "✗ bv MISSING"
command -v cass >/dev/null && echo "✓ cass" || echo "✗ cass MISSING"
command -v cm >/dev/null && echo "✓ cm" || echo "✗ cm MISSING"
command -v ubs >/dev/null && echo "✓ ubs" || echo "✗ ubs MISSING"
```

If any tool is missing, stop and direct to SETUP.md.

## 3. Read Project Context

```bash
cat AGENTS.md 2>/dev/null || echo "No AGENTS.md found"
cat CODEMAPS.md 2>/dev/null || echo "No CODEMAPS.md found"
```

## 4. Check Git State

```bash
git status --short
git log --oneline -5
```

## 5. Check Session History

```bash
cass search "$(basename $(pwd))" --robot --limit 3
```

Look for recent sessions and learn from them.

## 6. Get Learned Patterns

```bash
cm context "$(basename $(pwd))" --json 2>/dev/null || echo "No patterns yet"
```

## 7. Check Current State

```bash
ls .plans/*.md 2>/dev/null && echo "Plans exist" || echo "No plans"
ls .beads/ 2>/dev/null && echo "Beads exist" || echo "No beads"
```

## 8. Find Ready Tasks

```bash
bd ready --json 2>/dev/null || echo "No ready tasks"
```

## 9. Get AI Recommendations

```bash
bv --robot-triage 2>/dev/null || echo "No triage available"
```

## 10. Multi-Agent Check (If Available)

If Agent Mail MCP is available:
- Register with Agent Mail
- Check inbox for recent messages
- Discover other active agents

## Output Summary

After completing the checklist, output:

```
=== Session Initialized ===

Project: <name>
Phase: <planning | decomposition | implementation>
Ready Tasks: <count>
Recommended: <bead-id if available>

Context loaded. Ready to work.
```
