---
description: Show current workflow phase, tasks, and agent state
allowed-tools: Bash(bd:*), Bash(bv:*), Bash(ls:*), Bash(git:*), Read
---

# Workflow Status

Show the current state of the project workflow.

## Phase Detection

```bash
HAS_PLANS=$(ls .plans/*.md 2>/dev/null | wc -l)
HAS_BEADS=$(ls .beads/ 2>/dev/null | wc -l)
READY_BEADS=$(bd ready --json 2>/dev/null | grep -c '"id"' || echo 0)

if [ "$HAS_BEADS" -gt 0 ] && [ "$READY_BEADS" -gt 0 ]; then
  PHASE="implementation"
elif [ "$HAS_PLANS" -gt 0 ]; then
  PHASE="decomposition"
else
  PHASE="planning"
fi

echo "Current Phase: $PHASE"
```

## Plans Status

```bash
echo "=== Plans ==="
if ls .plans/*.md 2>/dev/null; then
  for f in .plans/*.md; do
    echo "- $(basename $f): $(head -1 $f | sed 's/^# //')"
  done
else
  echo "No plans yet"
fi
```

## Beads Status

```bash
echo "=== Beads ==="
if [ -d ".beads" ]; then
  bd list --json | jq -r '.[] | "\(.status): \(.id) - \(.title)"' | sort
  echo ""
  echo "Summary:"
  bd list --json | jq -r 'group_by(.status) | .[] | "\(.[0].status): \(length)"'
else
  echo "No beads yet"
fi
```

## Ready Tasks

```bash
echo "=== Ready to Start ==="
bd ready --json 2>/dev/null | jq -r '.[] | "- \(.id): \(.title)"' || echo "None"
```

## In Progress

```bash
echo "=== In Progress ==="
bd list --json 2>/dev/null | jq -r 'select(.status == "in_progress") | "- \(.id): \(.title) (\(.assignee))"' || echo "None"
```

## Blocked Tasks

```bash
echo "=== Blocked ==="
bd list --json 2>/dev/null | jq -r 'select(.blockedBy | length > 0) | "- \(.id): blocked by \(.blockedBy | join(", "))"' || echo "None"
```

## Git Status

```bash
echo "=== Git ==="
git status --short | head -10
echo "Branch: $(git branch --show-current)"
```

## Multi-Agent Status (If Available)

If Agent Mail MCP is available:
- List active agents
- Show recent messages
- Show file reservations

## Output Format

```
=== OMCC Status ===

Phase: <planning | decomposition | implementation>

Plans: <count>
Beads: <total> (<ready> ready, <in_progress> in progress, <completed> completed)

Recommended Next: <bead-id>

Git: <clean | X uncommitted changes>
Branch: <branch-name>
```
