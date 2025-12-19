# OMCC Setup Guide

Complete installation and configuration for the OMCC workflow.

## Prerequisites

- Claude Code CLI installed
- Git
- Bash shell
- curl

## Step 1: Install Required Tools

All 5 tools are **required**. The workflow will not function without them.

### Beads (bd + bv)

Task tracking with dependency graphs.

```bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/beads/main/install.sh | bash
```

Verify:
```bash
bd --version
bv --version
```

### CASS (Coding Agent Session Search)

Search past AI sessions for patterns and solutions.

```bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/coding_agent_session_search/main/install.sh | bash
```

Verify:
```bash
cass --version
```

### cass-memory (cm)

Cross-agent learning and pattern extraction.

```bash
# Linux
curl -L https://github.com/Dicklesworthstone/cass_memory_system/releases/latest/download/cm-linux-amd64 -o ~/.local/bin/cm

# macOS (Intel)
curl -L https://github.com/Dicklesworthstone/cass_memory_system/releases/latest/download/cm-macos-amd64 -o ~/.local/bin/cm

# macOS (Apple Silicon)
curl -L https://github.com/Dicklesworthstone/cass_memory_system/releases/latest/download/cm-macos-arm64 -o ~/.local/bin/cm

chmod +x ~/.local/bin/cm
```

Verify:
```bash
cm --version
```

### UBS (Ultimate Bug Scanner)

Pre-commit bug scanning with 1000+ patterns.

```bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/ultimate_bug_scanner/master/install.sh | bash
```

Verify:
```bash
ubs --version
```

## Step 2: Verify All Tools

Run this to confirm everything is installed:

```bash
echo "=== Tool Verification ===" && \
bd --version && echo "✓ bd" && \
bv --version && echo "✓ bv" && \
cass --version && echo "✓ cass" && \
cm --version && echo "✓ cm" && \
ubs --version && echo "✓ ubs" && \
echo "=== All tools installed ==="
```

## Step 3: Set Up Agent Mail (Optional - For Multi-Agent)

If you want multiple agents working simultaneously:

```bash
curl -fsSL https://raw.githubusercontent.com/Dicklesworthstone/mcp_agent_mail/main/scripts/install.sh | bash

# Add to Claude Code MCP config
claude mcp add agent-mail -- am server
```

## Step 4: Configure Your Project

### Option A: Copy Templates

```bash
cd your-project

# Copy skill templates
mkdir -p .claude/skills
cp -r /path/to/omcc/templates/skills/* .claude/skills/

# Copy other templates
cp /path/to/omcc/templates/project-claude.md CLAUDE.md
cp /path/to/omcc/templates/project-codemaps.md CODEMAPS.md

# Edit CLAUDE.md to match your project
$EDITOR CLAUDE.md
```

### Option B: Let Claude Do It (Recommended)

Tell Claude: "Set up OMCC from https://github.com/SebastiaanWouters/omcc"

Claude will auto-detect your project info and create a customized CLAUDE.md.

## Step 5: Initialize Beads

```bash
cd your-project
bd init
git add .beads/
git commit -m "Initialize OMCC workflow"
```

## Step 6: Index Sessions

```bash
cass index --full
```

This indexes past AI sessions for the learning system.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Tool not found | Add `~/.local/bin` to PATH: `export PATH="$HOME/.local/bin:$PATH"` |
| bv/cass hangs | Use `--robot` flags: `bv --robot-triage`, `cass search "x" --robot` |
| Permission denied | Run `chmod +x ~/.local/bin/{bd,bv,cass,cm,ubs}` |
| Agent Mail issues | Check with `am status`, restart with `am server restart` |

## Next Steps

1. Run `/prime` to initialize a session
2. Describe a feature to start planning
3. Or use `/plan <feature>` to explicitly start

See [README.md](README.md) for workflow overview.
