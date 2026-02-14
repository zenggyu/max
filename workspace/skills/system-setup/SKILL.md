---
name: system-setup
description: System environment setup and software installation for Linux and Windows. Use when user explicitly requests system setup, software installation, environment configuration, or configuration repair.
---

# System Setup

Principle-driven system software installation and configuration.

## Structure

## Workflow

## General Principles

1. **Official Sources Only** — Use sources explicitly endorsed by the software vendor, and follow official installation/troubleshooting guide.

   **Priority:** URL in software's .md file > general official documentation
   
   **GOOD:**
   - URL specified in software's .md file (highest priority)
   - Official vendor websites (nodejs.org, docker.com)
   - Official package repositories (vendor apt repos, Microsoft Store)
   - Official install scripts on vendor's verified domains
   - Official language registries (npmjs.com, pypi.org, crates.io)
   - Pause and ask user after official troubleshooting guide does not help
   
   **BAD:**
   - Random GitHub repos not owned by the vendor
   - Third-party PPAs without vendor endorsement
   - Unverified "mirror" sites
   - Search for solutions from random sources online, or implement custom hacks when installation or verification fails

2. **Prefer Simplicity** — Choose options that are isolated, stable, and have minimal dependencies.

   **GOOD:**
   - User-space installs: uv/venv for Python, nvm for Node.js, Scoop/Chocolatey on Windows
   - Installing binaries to `~/.local/bin` instead of `/usr/local/bin`
   - Storing configs in `~/.config/` instead of `/etc/`
   - Using apt on Ubuntu (included by default) over Homebrew (requires additional installation)
   - Latest stable version over old stable or unstable
   - Asking before privilege escalation (except `apt update/install`, system-wide services)
   
   **BAD:**
   - Using pip for system Python (poor dependency management, conflicts likely)
   - Adding new package managers when existing ones suffice
   - Installing global npm packages with sudo
   - Windows MSI installers requiring admin when user-space alternatives exist

3. **Trackable** — Maintain a single log file with complete records for all software installed.

   **Log location:** 
   - Linux: `/tmp/system-setup-<random>.log`
   - Windows: `%TEMP%\system-setup-<random>.log`
   
   Always inform user of the log file path.
   
   **Log template (repeat for each software):**
   
   | Field | Description |
   |-------|-------------|
   | SOFTWARE | Name of the software installed |
   | START_TIME | When installation began (YYYY-MM-DD HH:MM:SS) |
   | END_TIME | When installation completed (YYYY-MM-DD HH:MM:SS) |
   | SOURCE | URL used; explain if multiple options were available |
   | VERSION | Version installed; explain if multiple versions were available |
   | ACTION | Commands/method used; explain if multiple methods were available |
   | CONFIG | Configuration changes made post-install |
   | TEMP | Temporary changes; explain if any remain unreverted |
   | VERIFY | How functionality was verified; explain if FAILED |
   | RESULT | SUCCESS / FAILED / PARTIAL; explain if not SUCCESS |
   
   **Example:**
   ```
   SOFTWARE: uv
   START_TIME: 2024-02-14 15:30:00
   END_TIME: 2024-02-14 15:31:45
   SOURCE: https://docs.astral.sh/uv/getting-started/installation/ (official, recommended)
   VERSION: 0.5.24 (latest stable)
   ACTION: curl -LsSf https://astral.sh/uv/install.sh | sh (official install script)
   CONFIG: Added to PATH via ~/.bashrc
   TEMP: None
   VERIFY: uv --version returned 0.5.24
   RESULT: SUCCESS
   ```

4. **Be Clean** — Keep persistent changes minimal. Revert temporary changes.

   **GOOD:**
   - Only installing what was requested
   - Creating temporary test files in `/tmp/` that auto-clean
   - Reverting test configurations after verification
   - Documenting any unrevertable changes in the log
   
   **BAD:**
   - Installing "recommended" extras without asking
   - Leaving test databases or config files behind
   - Modifying system configs for temporary tests without reverting
   - Not documenting changes that couldn't be undone

5. **Verify Functionality** — Confirm the software works correctly.

   Each software's reference .md file defines what "functional" means for that software. The definition must be verifiable (e.g., a command that returns a non-error exit status).

6. **When Uncertain, Ask** — Pause and request user direction when:
   - In doubt about the correct approach
   - Principles conflict with each other
   - Software guide is ambiguous or incomplete
   - Multiple valid options with no clear preference
   - About to make irreversible changes
   - Verification fails and resolution is unclear

## Software-Specific Preferences

For each software to install, read its reference file in `references/software/<name>.md`. These files contain:
- Official URL(s) for: installation/trouble shooting guide
- User's preferences (which take precedence over general principles above)
- Post-installation configuration requirements

**Preference Priority:** Software .md file > SKILL.md principles > agent reasoning
