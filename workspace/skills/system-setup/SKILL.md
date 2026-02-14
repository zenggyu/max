---
name: system-setup
description: Ubuntu system environment setup and configuration. Use when setting up a fresh Ubuntu machine (server or personal computer), reinstalling software, repairing broken configurations, or configuring development/production environments. Handles software installation, configuration, template deployment, and state tracking for resume capability.
---

# System Setup

Automated Ubuntu system environment configuration for servers, workstations, and personal computers.

## Design Principles

1. **Dependency Awareness** — Auto-include dependencies; install in correct order without manual sequencing

2. **User Informed-Consent** — Present installation plan for approval before execution; user decides whether to proceed; pause and ask when encountering unexpected situations; never proceed with permission changes or security-related actions without explicit approval

3. **Robust & Transparent Progress** — Track progress per software visibly; continue with remaining software if one fails; safe to re-run (skips completed steps); interruption never requires starting over

### Unexpected Situations (Require Consent)

The following situations pause execution and ask for user direction:

1. **Errors & Failures** — Command fails, download fails → auto-retry three times for network timeout, then ask
2. **Permission/Security** — Requires sudo, adding PPA/GPG key, modifying sensitive files (`/etc/*`, `~/.ssh/*`) → ask immediately
3. **State Inconsistency** — State says installed but verification fails → ask
4. **Missing Resources** — Software guide, template, URL not found, inconsistent, or ambiguous → ask

## Workflow

### Phase 1: Discovery

1. **Detect OS**
   - Read `/etc/os-release`
   - Extract `ID`, `VERSION_ID`, and `VERSION_CODENAME`
   - If not Ubuntu → ask for confirmation

2. **Build Catalog**
   - List `.md` files in `references/software/`
   - Map available software

3. **Detect Status**
   - For each software: run verification command to check if installed and functional
   - "Functional" definition is in each software's `.md` file; if unfound → ask user
   - Mark which software needs installation / configuration

4. **Parse User Request**
   - Identify software to install
   - Handle "all", "resume", specific names

### Phase 2: Planning

1. **Collect Prerequisites**
   - Extract from **Prerequisites** section in `.md` files for all software marked for installation
   - This section must exist (even if empty: "Prerequisites: none")
   - If prerequisites not exist or ambiguous → pause and ask user, suggest updating the `.md` file, resume after the user replies and check again until clear
   - Distinguish which prerequisite can be handled without the user (installing or downloading dependencies with apt, npm, uv, uvx, curl, R INSTALL, etc.) and mark those to be executed (when in doubt, pause and ask)
   - Report other prerequisites to user; pause until confirmed ready

2. **Determine Install Order**
   - Topological sort based on prerequisites to be executed
   - Software with no prerequisites first
   - Dependents after their prerequisites
   - When in doubt, pause and ask user

3. **Report Plan**
   - Show ordered list: "Will install: A → B → C"
   - Note any skipped (already installed)

### Read State
```bash
STATE_FILE="/var/tmp/server-setup-state.json"
if [ -f "$STATE_FILE" ]; then
    STATE=$(cat "$STATE_FILE")
else
    STATE='{}'
fi
```

### Update State
```bash
# Update specific software status
echo "$STATE" | jq '.software.docker = {"installed": true, "configured": true}' > "$STATE_FILE"
```

### Reset State
```bash
rm -f /var/tmp/server-setup-state.json
```

## Reference Files

### OS Guides
- `references/os/ubuntu.md` — Ubuntu preparation and system updates

### Software Guides
Located in `references/software/`, one file per software:
- `docker.md` — Docker installation and configuration
- `git.md` — Git installation and configuration
- `nodejs.md` — Node.js installation and configuration
- `nginx.md` — Nginx installation and configuration
- etc.

Each software guide follows this structure:
```markdown
# Software Name

## Installation
Commands to install the software.

## Configuration
Commands to configure the software.

## Verification
Commands to verify installation.

## Notes
Any special considerations or troubleshooting.
```

### Templates
Located in `references/templates/`:
- `bashrc.template` — Shell configuration
- `gitconfig.template` — Git configuration
- `vimrc.template` — Vim configuration
- etc.

## Troubleshooting

**Permission denied:** If state file cannot be written to `/var/tmp/`, fall back to current directory.

**Software not found:** Check if the software `.md` file exists in `references/software/`.

**Partial failure:** State file tracks individual software status; resume from failed software.
