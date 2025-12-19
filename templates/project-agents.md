# AGENTS.md

This file defines the agentic workflow for this project.

## Workflow Phases

### Phase 1: Planning (`/plan <feature>`)
- Research and brainstorm approaches
- Create structured plan in `.plans/<feature>.md`
- Get user approval before proceeding

### Phase 2: Decomposition (`/decompose`)
- Break plan into beads (tasks) with dependencies
- Store in `.beads/` directory
- Each bead is independently executable

### Phase 3: Implementation (`/implement`)
- Execute beads in dependency order
- Validate with `ubs --staged` before commits
- Close beads with `bd close`

## Required Tools

All tools must be installed. Verify with:
```bash
bd --version && bv --version && cass --version && cm --version && ubs --version
```

| Tool | Purpose | Key Command |
|------|---------|-------------|
| bd | Task tracking | `bd ready --json` |
| bv | Graph analysis | `bv --robot-triage` |
| cass | Session search | `cass search "x" --robot` |
| cm | Pattern learning | `cm context "x" --json` |
| ubs | Bug scanning | `ubs --staged` |

**CRITICAL**: Always use `--json` or `--robot` flags. Bare commands will hang!

## Slash Commands

| Command | Purpose |
|---------|---------|
| `/plan <name>` | Start planning phase |
| `/decompose` | Break plan into beads |
| `/implement` | Start implementation |
| `/prime` | Initialize agent session |
| `/next-bead` | Find next task |
| `/status` | Show workflow state |

## Multi-Agent Coordination

When multiple agents work simultaneously:

1. **Claim ALL sub-beads** when claiming a parent
2. **Reserve files** before editing via Agent Mail
3. **Announce [CLAIMED]** when starting
4. **Announce [CLOSED]** when completing
5. **Check inbox** before claiming new work

## Handling New Requests

When user requests something during implementation:

| Request Type | Action |
|--------------|--------|
| Tiny & directly related (< 5 min) | Do immediately |
| Related to existing bead | Add to that bead |
| New work | Create new bead, continue current |
| Requires rearchitecting | Stop, discuss, potentially re-plan |

**Never** start large untracked work. Everything goes through beads.

## Safety Rules

- Never delete files without permission
- No destructive git commands without approval
- No bulk codemods or regex refactors
- Run `ubs --staged` before every commit

## Session Workflow

### Starting a Session
```bash
/prime
```

### During Work
1. `/next-bead` to find task
2. Implement the bead
3. `ubs --staged` to check
4. Commit and close bead
5. Repeat

### Ending a Session
- Ensure all in_progress beads are either completed or handed off
- Commit `.beads/` changes
