---
name: system-setup
description: System environment setup and configuration. Use when setting up a (fresh) machine (personal server or laptop; NOT FOR PRODUCTION) with Debian-based operating system. Handles software installation, configuration, template deployment.
---

# System Setup

Automated Debian-based system environment configuration for personal servers or laptops.

## Design Principles

- **Robust**:
    - Dependency-aware;
    - predictable
2. **Clean** - Never install anything beyond required; never create unnessary files; minimal impact on existing environment; auto-restore temporary changes

3. **Safe** — privacy aware, no leaking sensitive information, no permission bypass

### Situations Requiring User Direction

The following situations pause execution and ask for user direction:

1. **Errors & Failures** — Command fails, verification fails
    - Exception: for errors likely caused by network timeout, auto-retry 3 times before asking
2. **Permission/Security** — Requires sudo, adding PPA/GPG key, modifying sensitive files (`/etc/*`, `~/.ssh/*`, etc.), collecting/sending sensitive infomration (account, password, API key, email, phone number, .etc)
3. **Missing/Ambiguous/Inconsistent/Incomplete Resources**:
    - Software-specific Installation/configuration reference file (`references/*.md` file) or url
    - Templates
6. prerequisites
7. definition of functional
8. inconsistent
9. Others: see workflow and software.md file

## Workflow

### Phase 1: Discovery

1. **Clarify User Request**
   - Identify software to install
   - Handle "all", "resume", specific names
   - Ask user for preference: whether functional software should be reinstalled

2. **Detect OS**
   - Read `/etc/os-release`
   - Extract `ID`, `VERSION_ID`, and `VERSION_CODENAME`
   - If not Debian-based (Debian, Ubuntu, etc.) → ask for confirmation

3. **Build Catalog**
   - List `.md` files in `references/software/`
   - Map available software

4. **Detect Status**
   - For each software: check if it's already functional
   - The definition of "functional" should be defined in the **Definition of functional** section in each software's `.md` file
   - This section must exist and can provide verifiable output, if not found or unclear, pause and ask user, suggest updating the `.md` file, resume after the user replies and check again until clear
   - Mark which software needs installation (skip functional software unless user preferred)

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
