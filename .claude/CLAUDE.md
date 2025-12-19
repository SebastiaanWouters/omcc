# OMCC: Oh My Claude Code

This repository is a single source of truth for Claude Code workflows. Other projects reference this repo to get an auto-detecting, multi-phase development workflow.

## Purpose

Provides:
- Auto-detecting skills for Planning → Decomposition → Implementation
- Slash commands for explicit phase control
- Safety and coordination rules
- Templates for target repos

## Structure

```
.claude/
├── skills/          # Auto-detected phase workflows
├── commands/        # /slash commands
└── rules/           # Safety constraints

templates/           # Copy to target repos
docs/                # Philosophy and tool reference
```

## When Working on OMCC

This repo is for maintaining the workflow system itself. When editing:

1. Skills should have concise descriptions (auto-detection)
2. Commands should be explicit shortcuts
3. Templates should be project-agnostic
4. Docs should be reference material

## Testing Changes

To test changes, copy templates to a test project and verify:
- Skills auto-activate correctly
- Commands work as expected
- Tools are verified on startup
