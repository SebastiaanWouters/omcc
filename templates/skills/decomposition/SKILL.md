---
name: decomposition
description: |
  Decomposition workflow for breaking plans into beads. Activated when .plans/
  exists and user wants to create tasks. Also invoked via /decompose command.
---

# Decomposition Phase Workflow

You are now in the **Decomposition Phase**. Your goal is to break down the plan into executable beads (tasks) with proper dependencies.

## First: Verify Tools & State

```bash
command -v bd && command -v bv && command -v cass && command -v cm && command -v ubs
ls .plans/*.md 2>/dev/null || echo "No plan files found"
```

If no plan exists, guide user back to planning phase.

## Phase Detection Confirmation

You detected decomposition context because:
- `.plans/*.md` file(s) exist
- User mentioned "break down", "tasks", "decompose", or "ready to implement"
- Planning phase just completed and user agreed to proceed

**Acknowledge**: "I'll help decompose this plan into executable tasks."

## Step 1: Load & Present Plan

1. Read the plan file from `.plans/`
2. Extract the phases defined
3. Present a summary to user:

```
Plan: <Feature Name>

Phases identified:
1. Phase 1: <Name> - <Brief description>
2. Phase 2: <Name> - <Brief description>
...

Ready to create beads for each phase?
```

## Step 2: Initialize Beads (If Needed)

```bash
if [ ! -d ".beads" ]; then
  bd init
  echo "Initialized beads tracking"
fi
```

## Step 3: Interactive Bead Creation

For each phase in the plan, create a parent bead with sub-beads.

### Standard Sub-Bead Structure

| Suffix | Purpose | Content |
|--------|---------|---------|
| `.0` | Context Brief | WHY we're doing this, integration points, ADRs |
| `.1` | Schema/Types | Database schemas, TypeScript interfaces, API contracts |
| `.2` | Core Implementation | Main business logic |
| `.3` | Secondary Implementation | Supporting code, utilities |
| `.4` | Tests: Happy Path | Basic functionality tests |
| `.5` | Tests: Edge Cases | Boundary conditions, unusual inputs |
| `.6` | Tests: Error Handling | Failure modes, error recovery |
| `.7` | Tests: Integration | Cross-component tests |
| `.8` | Tests: E2E (if needed) | Full workflow tests |
| `.9` | Verification Checklist | Manual QA steps, review criteria |

### For Each Phase

1. **Propose Structure**
   ```
   Phase 1: User Authentication

   Proposed beads:
   - bd-001: User Authentication (parent)
     - bd-001.0: Context & Integration Points
     - bd-001.1: User Schema & Types
     - bd-001.2: Auth Service Implementation
     - bd-001.3: Session Management
     - bd-001.4: Tests: Login/Logout Happy Path
     - bd-001.5: Tests: Invalid Credentials
     - bd-001.6: Tests: Token Expiry
     - bd-001.9: Verification Checklist

   Does this breakdown make sense? Any adjustments?
   ```

2. **Get User Feedback** - Use AskUserQuestion if needed

3. **Create Beads** - For each approved bead:
   ```bash
   bd add --id bd-001 --title "User Authentication" --body "$(cat <<'EOF'
   ## Context
   <From plan>

   ## Scope
   <What this bead covers>

   ## Files
   - src/auth/...

   ## Acceptance Criteria
   - [ ] Criterion 1
   - [ ] Criterion 2
   EOF
   )"
   ```

## Step 4: Dependency Mapping

Ask user about dependencies:
- "Which beads must complete before others can start?"
- "Are there any parallel workstreams?"

Create dependencies:
```bash
bd update bd-001.2 --blocked-by bd-001.1
bd update bd-001.4 --blocked-by bd-001.2
```

Visualize the graph:
```bash
bv --robot-triage
```

Present dependency summary to user.

## Step 5: Lossless Transfer Verification

**CRITICAL**: Ensure every detail from the plan appears in a bead.

Checklist:
- [ ] All phases have parent beads
- [ ] All implementation details are in sub-beads
- [ ] All tests mentioned are captured as test beads
- [ ] All files to modify are listed in relevant beads
- [ ] All acceptance criteria are explicit

If anything is missing, create additional beads.

## Step 6: Final Review

Present complete bead structure:
```bash
bd list --json | jq '.[] | {id, title, status, blockedBy}'
```

Ask user:
- "Does this capture everything from the plan?"
- "Any beads too large? (Should be ~30-120 min of work)"
- "Any missing dependencies?"

## Step 7: Commit Beads

```bash
git add .beads/
git commit -m "Add beads for <feature-name>"
```

## Step 8: Transition to Implementation

Once beads are created:
- Show: "`X` beads created, `Y` ready to start"
- Offer: "Ready to start implementation?"

If user agrees, the implementation skill will automatically activate.

---

## Bead Content Guidelines

Each bead body should contain:

```markdown
## Context
Why this bead exists, how it fits into the larger feature.

## Scope
Exactly what this bead covers (and doesn't cover).

## Files
- `path/to/file.ts` - What changes

## Implementation Notes
Key decisions, patterns to follow, gotchas.

## Acceptance Criteria
- [ ] Specific, testable criterion
- [ ] Another criterion

## Dependencies
- Requires: bd-XXX (reason)
- Blocks: bd-YYY (reason)
```

## Token Efficiency

- Reference plan file instead of duplicating content
- Keep bead descriptions focused
- Use bullet points, not paragraphs
- Link to CODEMAPS.md for architecture context
