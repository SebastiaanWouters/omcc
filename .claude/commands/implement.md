---
description: Start the implementation phase to execute beads
allowed-tools: Bash(*), Read, Write, Edit, Grep, Glob, Task, AskUserQuestion
argument-hint: [bead-id | --multi]
---

# Start Implementation Phase

You are explicitly entering the **Implementation Phase**.

## Mode Detection

If `--multi` in arguments: Enable multi-agent mode
If bead-id provided: Start with that specific bead
Otherwise: Use AI prioritization to select first bead

## Invoke Implementation Skill

The implementation skill contains the full workflow. Execute it now with:
- User has explicitly requested implementation
- Skip auto-detection, go directly to task execution
- If `--multi`: Enable Agent Mail coordination

## Expected Output

- Claim beads
- Implement code
- Run `ubs --staged` before commits
- Close beads with `bd close`

Continue until user says stop or all beads complete.
