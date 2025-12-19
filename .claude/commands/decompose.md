---
description: Start the decomposition phase to break a plan into beads
allowed-tools: Bash(bd:*), Bash(bv:*), Bash(ls:*), Bash(cat:*), Read, Write, AskUserQuestion
argument-hint: [plan-file]
---

# Start Decomposition Phase

You are explicitly entering the **Decomposition Phase**.

## Find Plan

If plan file provided: $ARGUMENTS
Otherwise, find the most recent plan:
```bash
ls -t .plans/*.md 2>/dev/null | head -1
```

## Invoke Decomposition Skill

The decomposition skill contains the full workflow. Execute it now with:
- Plan file to decompose
- User has explicitly requested decomposition
- Skip auto-detection, go directly to bead creation

## Expected Output

- Initialize `.beads/` if needed
- Create beads with `bd add`
- Map dependencies

When complete, ask: "Beads ready. Want to start implementation?"
