# Multi-Agent Coordination Rules

These rules prevent conflicts when multiple agents work simultaneously.

## Rule 1: Claim All Sub-Beads Together

When claiming a parent bead, you MUST claim ALL its sub-beads:

```bash
# Correct ✓
bd update bd-001 --status in_progress --assignee "Agent"
bd update bd-001.0 --status in_progress --assignee "Agent"
bd update bd-001.1 --status in_progress --assignee "Agent"
bd update bd-001.2 --status in_progress --assignee "Agent"
# ... ALL sub-beads

# Wrong ✗
bd update bd-001 --status in_progress --assignee "Agent"
# (leaves sub-beads unclaimed → other agents see them as "ready")
```

**Why**: Unclaimed sub-beads appear available to other agents, causing conflicts.

## Rule 2: Check Reservations Before Editing

Before editing ANY file, check if another agent has reserved it:

```python
# Check current reservations
file_reservation_paths(project_key, agent_name, paths=[], ttl_seconds=0)
```

If a file is reserved by another agent:
1. **DO NOT EDIT IT**
2. Check inbox for coordination messages
3. Wait or work on something else

## Rule 3: Reserve Files Before Work

Before starting work on a bead:

```python
file_reservation_paths(
    project_key,
    agent_name,
    paths=["src/module/**", "tests/test_module.py"],
    ttl_seconds=3600,
    exclusive=True,
    reason="bd-XXX"
)
```

- Use glob patterns for directories
- Set reasonable TTL (1 hour typical)
- Include test files

## Rule 4: Announce Claims and Completions

### When Claiming

```python
send_message(
    project_key,
    sender_name,
    to=["all"],
    subject="[CLAIMED] bd-XXX - Feature Title",
    body_md="Starting **bd-XXX**.\n\nFile reservations:\n- `src/module/**`\n- `tests/test_module.py`",
    thread_id="bd-XXX"
)
```

### When Completing

```python
send_message(
    project_key,
    sender_name,
    to=["all"],
    subject="[CLOSED] bd-XXX - Feature Title",
    body_md="Completed **bd-XXX**.\n\nFiles changed:\n- `src/module/auth.ts`\n\nReleasing reservations.",
    thread_id="bd-XXX"
)
```

## Rule 5: Check Inbox Before Claiming

Before claiming new work:

```python
list_messages(project_key, since_minutes=30)
```

Look for:
- Recent [CLAIMED] messages (avoid duplicating work)
- Questions from other agents
- Coordination requests

## Rule 6: Handle Conflicts Gracefully

If you discover a conflict:

1. **STOP immediately**
2. Do not save any conflicting changes
3. Send message to the other agent:
   ```python
   send_message(
       project_key,
       sender_name,
       to=["conflicting-agent"],
       subject="[CONFLICT] File overlap on bd-XXX",
       body_md="I was about to edit `src/file.ts` but see you have it reserved. How should we coordinate?",
       thread_id="conflict-XXX"
   )
   ```
4. Wait for resolution

## Rule 7: Release Reservations Promptly

When done with a bead:

```python
file_reservation_paths(
    project_key,
    agent_name,
    paths=["src/module/**"],  # Same paths you reserved
    ttl_seconds=0  # 0 = release immediately
)
```

Don't hold reservations longer than needed.

## Rule 8: Respect the Dependency Graph

Never start work on a blocked bead:

```bash
# Check if blocked
bd show bd-XXX --json | jq '.blockedBy'
```

If `blockedBy` is non-empty:
1. Work on the blocking beads first
2. Or coordinate with the agent working on blockers

## Emergency Protocol

If coordination fails:
1. Stop all work
2. Notify all agents: `send_message(..., subject="[EMERGENCY] Coordination breakdown")`
3. Wait for human intervention
