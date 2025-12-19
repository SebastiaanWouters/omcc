---
description: Find and safely claim the next task to work on
allowed-tools: Bash(bd:*), Bash(bv:*), Bash(git:*), Read
argument-hint: [bead-id]
---

# Find Next Bead

Find the best next task to work on, checking for conflicts.

## If Bead ID Provided

If you were given a specific bead ID: $ARGUMENTS

1. **Verify it's available**
   ```bash
   bd show $1 --json
   ```

2. **Check for conflicts**
   - Is it already in_progress?
   - Is it blocked by uncompleted beads?

3. **If available, claim it** (see claiming protocol below)

## If No Bead ID Provided

### Step 1: Close Previous Work

If you have in_progress beads, close them first:
```bash
bd list --json | grep -A5 '"status": "in_progress"'
```

### Step 2: Get AI Recommendations

```bash
bv --robot-triage
```

Analyze:
- Unblocked beads (ready to start)
- High PageRank (unblocks others)
- High betweenness (critical path)

### Step 3: Check Ready Tasks

```bash
bd ready --json
```

### Step 4: Multi-Agent Conflict Check

If Agent Mail is available:
1. Check inbox for recent [CLAIMED] messages
2. Check file reservations
3. Discover other active agents

### Step 5: Select Best Task

Choose based on:
1. Not already claimed by another agent
2. Has highest impact (unblocks most work)
3. Files don't overlap with other agents' reservations

## Claiming Protocol

### For Single-Agent Mode

```bash
bd update <bead-id> --status in_progress --assignee "Claude"
```

If parent bead, claim ALL sub-beads:
```bash
bd update <bead-id>.0 --status in_progress --assignee "Claude"
bd update <bead-id>.1 --status in_progress --assignee "Claude"
# ... etc
```

### For Multi-Agent Mode

1. **Reserve Files First**
   ```python
   file_reservation_paths(
       project_key, agent_name,
       paths=["src/affected/**"],
       ttl_seconds=3600,
       exclusive=True,
       reason="<bead-id>"
   )
   ```

2. **Claim All Sub-Beads**
   ```bash
   bd update <bead-id> --status in_progress --assignee "<agent-name>"
   # ... all sub-beads
   ```

3. **Announce [CLAIMED]**
   ```python
   send_message(
       project_key, sender_name,
       to=["all"],
       subject="[CLAIMED] <bead-id> - <title>",
       body_md="Starting work on <bead-id>...",
       thread_id="<bead-id>"
   )
   ```

## Output

```
=== Next Bead ===

Claimed: <bead-id>
Title: <title>
Scope: <brief description>
Files: <list of files>

Ready to implement.
```
