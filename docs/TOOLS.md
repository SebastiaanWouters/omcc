# OMCC Tools Reference

Complete reference for the 5 required tools.

## Critical Rule

**NEVER** run bare `bv` or `cass` commands. They have TUI modes that hang agents.

```bash
# WRONG - Will hang
bv
cass search "query"

# CORRECT - Use robot/json flags
bv --robot-triage
cass search "query" --robot
```

---

## bd (Beads)

Task tracking with dependency support.

### Common Commands

```bash
# Initialize in project
bd init

# Add a bead
bd add --id bd-001 --title "Feature Title" --body "Description"

# List all beads
bd list --json

# Show ready (unblocked) beads
bd ready --json

# Update bead status
bd update bd-001 --status in_progress --assignee "Agent"

# Add dependency
bd update bd-002 --blocked-by bd-001

# Close a bead
bd close bd-001 --reason "Completed implementation"

# Show bead details
bd show bd-001 --json
```

### Bead States

| State | Meaning |
|-------|---------|
| pending | Not started |
| in_progress | Being worked on |
| completed | Done |
| blocked | Waiting on dependencies |

---

## bv (Beads Viewer)

Graph analysis and prioritization.

### Common Commands

```bash
# Get AI triage (recommended)
bv --robot-triage

# Get next best task
bv --robot-next

# Get graph metrics
bv --robot-plan
```

### Metrics Explained

| Metric | Meaning |
|--------|---------|
| PageRank | How many beads this unblocks (higher = more impact) |
| Betweenness | How critical this is to the overall flow |
| Ready | Can be started immediately |

---

## cass (Coding Agent Session Search)

Search past AI sessions.

### Common Commands

```bash
# Search sessions
cass search "authentication" --robot --limit 5

# Index all sessions (run periodically)
cass index --full

# Search with JSON output
cass search "bug fix" --json
```

### What It Indexes

- Claude Code sessions
- Cursor sessions
- Aider sessions
- Copilot sessions
- Other AI coding tools

---

## cm (cass-memory)

Pattern learning from past sessions.

### Common Commands

```bash
# Get context for a task
cm context "database migration" --json

# Get rules learned
cm rules --json

# Get antipatterns to avoid
cm antipatterns --json
```

### Output Structure

```json
{
  "rules": ["Use transactions for migrations", ...],
  "antipatterns": ["Don't use raw SQL in controllers", ...],
  "relevant_sessions": [...]
}
```

---

## ubs (Ultimate Bug Scanner)

Pre-commit bug scanning.

### Common Commands

```bash
# Scan staged files (use before commit)
ubs --staged

# Fail on warnings (stricter)
ubs --staged --fail-on-warning

# Scan specific file
ubs path/to/file.ts

# Scan directory
ubs src/
```

### Pattern Categories

- Security issues (XSS, injection, secrets)
- Logic errors (off-by-one, null refs)
- Performance issues (N+1, memory leaks)
- Style issues (dead code, complexity)

### Integration

Always run before commit:
```bash
ubs --staged && git commit -m "message"
```

---

## Tool Combinations

### Starting Work

```bash
# Get context
cm context "task description" --json

# Find prior solutions
cass search "similar problem" --robot --limit 3

# Get task recommendations
bv --robot-triage
```

### Claiming Work

```bash
# Get ready tasks
bd ready --json

# Claim task
bd update bd-001 --status in_progress --assignee "Agent"
```

### Completing Work

```bash
# Validate
ubs --staged

# Close
bd close bd-001 --reason "Implemented feature"

# Commit
git add -A && git commit -m "message"
```

### Multi-Agent Coordination

If Agent Mail is available:

```python
# Register
register_agent(project_key, personality="focused")

# Reserve files
file_reservation_paths(project_key, agent_name, paths=["src/**"], exclusive=True)

# Announce
send_message(project_key, agent_name, to=["all"], subject="[CLAIMED] bd-001")

# Check inbox
list_messages(project_key, since_minutes=30)
```

---

## Troubleshooting

### bd: Command not found

```bash
export PATH="$HOME/.local/bin:$PATH"
```

### bv hangs

You ran bare `bv`. Kill it and use:
```bash
bv --robot-triage
```

### cass: No results

Run indexing:
```bash
cass index --full
```

### cm: Empty context

Build up history first. cm learns from past sessions.

### ubs: Too many warnings

Focus on errors first:
```bash
ubs --staged --errors-only
```
