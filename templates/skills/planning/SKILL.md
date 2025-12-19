---
name: planning
description: |
  Planning workflow for new features. Activated when discussing new features,
  architecture, or roadmaps. Also invoked via /plan command. Produces .plans/*.md.
---

# Planning Phase Workflow

You are now in the **Planning Phase**. Your goal is to guide the user through comprehensive planning and produce a structured plan file.

## First: Verify Tools

Before proceeding, verify required tools are installed:
```bash
command -v bd && command -v bv && command -v cass && command -v cm && command -v ubs
```
If any tool is missing, stop and direct user to SETUP.md.

## Phase Detection Confirmation

You detected planning context because:
- User is describing a new feature or problem to solve
- User is asking "how should we build X"
- No `.plans/` or `.beads/` exist for this feature
- User explicitly mentioned planning, roadmapping, or architecture

**Acknowledge**: "I'll help you plan this. Let me gather some context first."

## Step 1: Gather Context (Do This in Parallel)

Launch parallel operations to understand the codebase:

1. **Explore Codebase** - Spawn Explore agents to find:
   - Related existing code
   - Patterns and conventions used
   - Files that will likely be affected

2. **Check Prior Work** - Run:
   ```bash
   cass search "relevant-keywords" --robot --limit 5
   ```
   Look for past sessions that solved similar problems.

3. **Get Learned Patterns** - Run:
   ```bash
   cm context "feature description" --json
   ```
   Extract rules and antipatterns from past work.

4. **Read CODEMAPS.md** - If it exists, understand current architecture.

## Step 2: Ask Clarifying Questions

Use AskUserQuestion to resolve ambiguities:
- What is the core problem being solved?
- Who are the users/consumers of this feature?
- What are the constraints (performance, compatibility, etc.)?
- Are there specific technologies or patterns to use/avoid?

**Do not proceed until you understand the requirements clearly.**

## Step 3: Brainstorm with Extended Thinking

Enable deep analysis mode. Consider:

1. **Multiple Approaches** - Identify 2-3 viable solutions
2. **Trade-offs** - Performance, complexity, maintainability
3. **Risks** - What could go wrong? Edge cases?
4. **Dependencies** - What needs to exist first?

Present options to user with clear trade-offs. Ask which approach they prefer.

## Step 4: Research Best Practices

Use WebSearch to find:
- Current best practices for this type of feature
- Common pitfalls to avoid
- Library/framework recommendations

Apply learned patterns from `cm context` output.

## Step 5: Update/Create Codemap

If this feature changes architecture:
1. Read existing CODEMAPS.md
2. Add new components/relationships
3. Create Mermaid diagrams for data flow

## Step 6: Produce the Plan

Create the plan file at `.plans/<feature-name>.md` with this structure:

```markdown
# Plan: <Feature Name>

## Problem Statement
What problem are we solving? Why does it matter?

## Goals
- [ ] Goal 1
- [ ] Goal 2
- [ ] Goal 3

## Non-Goals
What are we explicitly NOT doing?

## Approach
Describe the chosen approach and why it was selected.

### Key Decisions
- Decision 1: Rationale
- Decision 2: Rationale

## Phases

### Phase 1: <Name>
**Scope**: What gets built
**Files Affected**:
- `path/to/file1.ts` - Description of changes
- `path/to/file2.ts` - Description of changes

**Details**:
<Detailed implementation notes>

### Phase 2: <Name>
...

## Testing Strategy
How will we verify this works?

## Risks & Mitigations
| Risk | Mitigation |
|------|------------|
| Risk 1 | How we handle it |

## Open Questions
- Question 1?
- Question 2?
```

## Step 7: Review with User

Present the plan summary. Ask:
- "Does this capture your requirements?"
- "Any phases missing or out of order?"
- "Questions about the approach?"

Make adjustments based on feedback.

## Step 8: Transition to Decomposition

Once the plan is approved:
- Confirm: "Plan complete and saved to `.plans/<feature>.md`"
- Offer: "Ready to decompose into executable tasks?"

If user agrees, the decomposition skill will automatically activate.

---

## Output Format

Always create `.plans/` directory if it doesn't exist:
```bash
mkdir -p .plans
```

Plan files should be named with kebab-case: `.plans/user-authentication.md`

## Token Efficiency

- Use CODEMAPS.md instead of reading many source files
- Summarize exploration results, don't dump raw output
- Focus on decisions and rationale, not implementation details
