# OMCC Philosophy

Why this workflow exists and how it solves real problems in AI-assisted development.

## The Problems

### 1. Amnesia

AI agents forget context across sessions. What you discussed yesterday is gone today.

**Solution**: Beads persist task context. CASS indexes sessions. cm extracts patterns.

### 2. No Visibility

You can't see task dependencies or project impact. Work happens in a black box.

**Solution**: bv provides graph analysis with PageRank and betweenness metrics. You see what blocks what.

### 3. Agent Conflicts

Multiple agents overwrite each other's work. Merge conflicts, lost changes.

**Solution**: Agent Mail provides file locks, messaging, and presence tracking.

### 4. Lost Knowledge

Past solutions disappear. You solve the same problems repeatedly.

**Solution**: cass-memory (cm) distills sessions into rules and antipatterns.

### 5. Undetected Bugs

AI-generated code has subtle issues. You find them in production.

**Solution**: UBS scans for 1000+ bug patterns before commit.

### 6. Slow Search

Serial searches pollute context. You spend tokens on exploration.

**Solution**: CODEMAPS.md provides compressed architecture context. cm provides patterns.

## The 3-Phase Framework

### Phase 1: Planning

**Goal**: Understand the problem before writing code.

- Research existing patterns
- Brainstorm approaches with extended thinking
- Get user approval on direction
- Document decisions

**Output**: `.plans/<feature>.md`

### Phase 2: Decomposition

**Goal**: Create executable units of work.

- Break plan into beads (tasks)
- Map dependencies
- Ensure lossless transfer from plan
- Each bead independently executable

**Output**: `.beads/` structure

### Phase 3: Implementation

**Goal**: Execute efficiently with validation.

- AI-prioritized task selection
- Claim and coordinate (multi-agent)
- Validate before commit
- Close and document

**Output**: Working code

## Why This Order Matters

```
Planning BEFORE Decomposition
  → Ensures you build the right thing
  → Catches architectural issues early
  → Gets stakeholder buy-in

Decomposition BEFORE Implementation
  → Enables parallelization
  → Tracks progress visibly
  → Prevents scope creep

Validation DURING Implementation
  → Catches bugs early
  → Maintains quality
  → Documents as you go
```

## Extended Thinking (Ultrathink)

Use deep analysis for:
- Complex algorithm design
- Architectural decisions
- Debugging difficult issues
- Multi-file refactoring

Don't use for:
- Simple edits
- Well-understood patterns
- Straightforward implementations

## Token Efficiency

### Do
- Read CODEMAPS.md first
- Use cm for patterns
- Use --json flags
- Focus on bead scope

### Don't
- Read every file
- Duplicate context
- Use verbose outputs
- Explore beyond scope

## Multi-Agent Philosophy

### Single Agent (Default)
- Simple, predictable
- Full context in one place
- No coordination overhead

### Multi-Agent (When Needed)
- Parallel work on independent beads
- Mandatory coordination protocols
- Explicit file reservations

**Rule**: Start single, go multi when you have:
- 5+ independent beads
- Clear file boundaries
- Agent Mail configured

## New Requests During Work

### Do
- Track everything in beads
- Add to existing beads when related
- Create new beads for new work
- Communicate what you're doing

### Don't
- Start untracked work
- Mix unrelated changes
- Forget to document decisions
- Assume user knows what happened

## Key Principles

1. **Everything is a bead** - No untracked work
2. **Plan before code** - Understand before implementing
3. **Validate always** - ubs before every commit
4. **Communicate** - Tell user what you're doing
5. **Document decisions** - Future you will thank you
