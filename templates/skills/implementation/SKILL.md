---
name: implementation
description: |
  Implementation workflow for executing beads. Activated when .beads/ has ready
  tasks and user wants to code. Also invoked via /implement command.
---

# Implementation Phase Workflow

You are now in the **Implementation Phase**. Your goal is to execute beads efficiently, either alone or coordinating with other agents.

## First: Verify Tools & State

```bash
command -v bd && command -v bv && command -v cass && command -v cm && command -v ubs
ls .beads/ 2>/dev/null || echo "No beads directory"
bd ready --json | head -20
```

If no beads exist, guide user to planning/decomposition phase.

## Phase Detection Confirmation

You detected implementation context because:
- `.beads/` directory exists with ready tasks
- User mentioned "implement", "code", "build", or "start working"
- Decomposition phase completed and user agreed to proceed

**Acknowledge**: "I see X tasks ready. Let me start with the highest priority."

---

## Single-Agent Mode (Default)

### Step 1: Initialize Session

1. **Read Context**
   ```bash
   cat CLAUDE.md 2>/dev/null || echo "No CLAUDE.md"
   cat CODEMAPS.md 2>/dev/null || echo "No CODEMAPS.md"
   ```

2. **Check Session History**
   ```bash
   cass search "project-name" --robot --limit 3
   ```

3. **Get Patterns**
   ```bash
   cm context "implementation" --json
   ```

### Step 2: Select Task (AI-Prioritized)

```bash
bv --robot-triage
```

Analyze the output:
- Which beads are unblocked?
- Which have highest PageRank (unblock others)?
- Which have highest betweenness (critical path)?

**Present to user**: "I recommend starting with [bead-id]: [title]. It unblocks X other tasks."

### Step 3: Claim Task

```bash
bd update <bead-id> --status in_progress --assignee "Claude"
```

If this is a parent bead, claim ALL sub-beads:
```bash
bd update <bead-id>.0 --status in_progress --assignee "Claude"
bd update <bead-id>.1 --status in_progress --assignee "Claude"
# ... all sub-beads
```

### Step 4: Implement

Use extended thinking for complex logic.

**Workflow**:
1. Read the bead body for context and acceptance criteria
2. Read relevant source files
3. Write tests first (TDD when applicable)
4. Implement to make tests pass
5. Make incremental commits

**Best Practices**:
- Follow existing code patterns (check cm context)
- Keep changes focused on bead scope
- Ask user if uncertain: "This change affects X. Should I proceed?"

### Step 5: Validate

Before completing:

1. **Run Bug Scanner**
   ```bash
   ubs --staged
   ```
   Fix any issues found.

2. **Run Tests**
   ```bash
   # Project-specific test command
   npm test  # or pytest, cargo test, etc.
   ```

3. **Manual Check**
   - Does the code meet acceptance criteria?
   - Any edge cases missed?

### Step 6: Complete Task

```bash
bd close <bead-id> --reason "Implemented <brief description>"
git add -A && git commit -m "<meaningful message>"
```

### Step 7: Continue Loop

- "Task complete. Ready for the next one?"
- Return to Step 2

---

## Optional: Multi-Agent Mode

> **Note**: This section is optional. Skip if you're working alone or don't have Agent Mail MCP configured.

**Activated when**: Agent Mail MCP is available AND user says "spawn agents" or uses `/implement --multi`

### Agent Mail Setup Check

Test if Agent Mail MCP is available by checking for these functions:
- `register_agent`
- `send_message`
- `list_messages`
- `file_reservation_paths`

If not available, fall back to single-agent mode.

### Agent Startup Protocol

1. **Register with Agent Mail**
   ```python
   register_agent(project_key="project-name", personality="focused")
   # Returns: agent_name (e.g., "focused-fox-42")
   ```

2. **Check Inbox**
   ```python
   list_messages(project_key, since_minutes=60)
   ```
   Look for recent `[CLAIMED]` messages to avoid conflicts.

3. **Check File Reservations**
   ```python
   file_reservation_paths(project_key, agent_name, paths=[], ttl_seconds=0)
   # With empty paths, returns current reservations
   ```

4. **Get Task Recommendations**
   ```bash
   bv --robot-triage
   ```

### Before Claiming a Bead

**MANDATORY STEPS**:

1. **Verify No Conflicts**
   - Check if bead is already claimed (bd list --json)
   - Check inbox for [CLAIMED] on this bead
   - Check file reservations for overlapping paths

2. **Reserve Files**
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

3. **Claim All Sub-Beads**
   ```bash
   bd update bd-XXX --status in_progress --assignee "agent-name"
   bd update bd-XXX.0 --status in_progress --assignee "agent-name"
   bd update bd-XXX.1 --status in_progress --assignee "agent-name"
   # ALL sub-beads must be claimed
   ```

4. **Announce Claim**
   ```python
   send_message(
       project_key,
       sender_name,
       to=["all"],
       subject="[CLAIMED] bd-XXX - Feature Title",
       body_md="Starting **bd-XXX** (plus sub-beads .0-.9).\n\nFile reservations: `src/module/**`",
       thread_id="bd-XXX"
   )
   ```

### After Completing a Bead

1. **Release Reservations**
   ```python
   file_reservation_paths(
       project_key,
       agent_name,
       paths=["src/module/**"],
       ttl_seconds=0  # Release immediately
   )
   ```

2. **Announce Completion**
   ```python
   send_message(
       project_key,
       sender_name,
       to=["all"],
       subject="[CLOSED] bd-XXX - Feature Title",
       body_md="Completed **bd-XXX**.\n\nFiles changed: `src/module/auth.ts`, `tests/auth.test.ts`\n\nReleasing reservations.",
       thread_id="bd-XXX"
   )
   ```

3. **Close Bead**
   ```bash
   bd close bd-XXX --reason "Completed implementation"
   ```

4. **Check Inbox**
   Look for coordination requests from other agents.

### Conflict Prevention Rules

- **NEVER** edit files another agent has reserved
- **ALWAYS** claim parent + all sub-beads together
- **ALWAYS** check inbox before claiming new work
- **ALWAYS** announce changes that affect shared code
- **IF CONFLICT**: Stop, send message to conflicting agent, wait for resolution

---

## Extended Thinking Usage

Enable deep analysis for:
- Complex algorithm implementation
- Multi-file refactoring
- Debugging difficult issues
- Architectural decisions

Trigger by prefacing complex tasks with thorough analysis.

---

## Tool Command Reference

**CRITICAL**: Always use robot/json flags to avoid TUI hangs.

| Tool | Correct | WRONG (hangs) |
|------|---------|---------------|
| bd | `bd ready --json` | `bd ready` |
| bv | `bv --robot-triage` | `bv` |
| cass | `cass search "x" --robot` | `cass search "x"` |
| cm | `cm context "x" --json` | `cm context "x"` |
| ubs | `ubs --staged` | N/A |

---

## Handling New Requests During Implementation

When user requests something new while you're implementing:

### 1. Evaluate the Request

Ask yourself:
- Is this directly related to current bead scope?
- Is this a tiny fix (< 5 minutes)?
- Does this require architectural changes?

### 2. Decision Tree

```
New request arrives
       │
       ▼
Is it < 5 min AND directly related to current bead?
       │
   ┌───┴───┐
   Yes     No
   │       │
   ▼       ▼
Do it   Is it related to an existing bead?
now         │
        ┌───┴───┐
        Yes     No
        │       │
        ▼       ▼
   Add to    Create new bead
   existing  with bd add
   bead
```

### 3. Actions

**Tiny & Related** → Do immediately, document in current bead close reason

**Related to Existing Bead** → Add to that bead:
```bash
bd update <existing-bead> --body "$(bd show <existing-bead> --json | jq -r '.body')

## Additional Request
<new requirement>"
```

**New Work** → Create a bead:
```bash
bd add --id bd-NEW --title "New Request: <title>" --body "<description>"
```
Then continue current work, tackle new bead later.

### 4. Communicate

Always tell user what you're doing:
- "I'll add this to bead bd-XXX and handle it when I get there."
- "This needs a new bead. I'll create bd-YYY and continue current work."
- "This is quick and related, I'll do it now."

---

## Token Efficiency

- Read CODEMAPS.md before diving into source files
- Use `cm context` to get patterns instead of searching
- Summarize bead content, don't repeat entire body
- Batch related file reads
- Focus on files in bead scope, ignore others
