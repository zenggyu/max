---
name: system-setup
description: Guide for system software installation and configuration. Use whenever software installation or environment configuration is required (by user explicit request or agent initiative).
---

# System Setup

This skill provides guidance for system software installation and configuration.

## Structure

- `SKILL.md` — General workflow and preferences. Read first for all installation and configuration tasks.
- `references/*.md` — Per-software info, including official URLs, installation/configuration instructions, verification criteria. Read as needed for software-specific task.

## General Workflow

1. **Receive requests** — User requests or agent initiates software installation or environment configuration.
2. **Check references** — Search `references/` for relevant reference file(s) and read.
    - do not read irrelevant reference files
    - if no reference is found and the change has system/user-level impact and is persistent (as opposed to local or temporary), stick with the general guidance provided by this skill, be extra careful in the following steps, and ask user proactively.
3. **Plan** — Choose installation/configuration method:
    - check system information (CPU architecture, operating system version, etc.)
    - choose installation/configuration method based on system information, software-specific preferences, and general guidance.
4. **Execute** — Perform the installation/configuration actions:
    - when errors or unexpected issues occur, first consult the official documentation for solution (use the url provided in the reference file)
    - if the official url is not provided or does not help, ask user for guidance
    - do not implement custom hacks or workarounds without user approval
    - do not trust unofficial sources or unverified solutions
5. **Verify** — Confirm installation/configuration is successful:
    - follow verification steps in the reference file
    - if reference file does not exist or does not provide verification steps, perform basic checks to ensure software is functional
    - if the verification step produces test files or results in changes only intended for verification, clean them up afterwards
6. **Log and report** — Record all operations and summarised results, and put them in a report under a temporary directory (e.g., `/tmp` for Linux).

Repeat steps 2-6 for each installation/configuration task.

## General Preferences

### Source of Truth

In order of priority (from high to low; when in conflict, follow the higher priority source):

- User instruction
- Software-specific reference files: `references/*.md`
- General guidance: `SKILL.md`
- Official sources ("official" means directly provided or explicitly endorsed by user or software vendor; watch out for impersonation)

Never trust anything except the sources listed above, unless user explicitly approves. For example, information from search engines, forums, or social media should not be trusted without approval. Do not blindly follow links that claim to be official.

Examples of official sources:

- URLs specified in user prompt or `references/*.md`
- Well known vendor websites  (`https://cran.r-project.org/`, `https://www.python.org/`, etc.) and repositories (`docker.io`, `https://cloud.r-project.org/bin/`, `https://github.com/openclaw/openclaw`, etc.) 
- It's okay to use mirror sites provided by authoritive organizations (e.g., university/big company owned mirror sites like `https://mirrors.tuna.tsinghua.edu.cn/CRAN`, or `https://mirrors.aliyun.com/pypi/`), but only when the primary source is unavailable or too slow; ask user if unsure about whether an organization should be considered "authoritive"

### Be Appropriately Conservative

Choose options that are up-to-date, robust, and have minimal requirements/impact:

- When choosing software versions, prefer the latest stable version over the latest unstable or old stable ones
- Prefer user-level (e.g., under `~/`, or use virtual environments) over system-level installation and configuration
- Prefer modern package managers that does a better job on dependency management, isolation, and simplifies installation (e.g., `uv` over `pip`, `bun` over `npm`)
- Prefer solutions that present lower risk (e.g., `podman` rootless container over `docker` root container)
- Prefer solutions that are more widely adopted and have better community support (e.g., `apt` over `snap` on Ubuntu)
- Prefer existing tools and workflows over introducing new ones (e.g., `apt` over `flatpak` on Ubuntu), unless there is a good reason to do otherwise (e.g., if a software is available both from `uv` and `apt`, prefer `uv` because it offers better isolation and dependency management)
- Prefer containerized solutions for services like databases, web servers, and other complex applications (exception: simple services or client-side tools like `psql`)
- General preference order of package managers: language-specific (`uv`, `bun`, .etc) > user-level (`homebrew`, `chocolatey`, .etc) > system (`apt`, `yum`, .etc) > universal (`snap`, `flatpak`, .etc)
- Create backups before deleting or modifying existing files
- When uncertain, ask for user guidance

### Be Clean

Keep persistent changes minimal:

- Never install anything beyond required, unless explicitly approved by the user
- Revert temporary changes (environment variables, configurations, processes, etc.) for verification after use
- Temporary files should be created in `/tmp/` on Linux or `%TEMP%` on Windows and deleted after use

### Be Transparent

Create a report for each software installation task:

- Report location:
   - Linux: `/tmp/system-setup-<yyyymmddHHMMSS>-<random>.log`
   - Windows: `%TEMP%\system-setup-<yyyymmddHHMMSS>-<random>.log`\
- This report should be created at the beginning of the task, and maintained during the installation process
- Inform user about the report after the task completes or aborts
- Use report template (see below)

Report template (repeat for each software):

| Field | Description |
|-------|-------------|
| SOFTWARE | Name of the software installed |
| START_TIME | When installation began (YYYY-MM-DD HH:MM:SS) |
| END_TIME | When installation completed (YYYY-MM-DD HH:MM:SS) |
| SOURCE | URL used; explain if multiple options were available |
| VERSION | Version installed; explain if multiple versions were available |
| ACTIONS | Commands/method used; explain if multiple methods were available |
| CONFIG | Configuration changes made post-install |
| TEMP | Temporary changes; explain if any remain unreverted |
| VERIFY | How functionality was verified; explain if FAILED |
| RESULT | SUCCESS / FAILED / PARTIAL; explain if not SUCCESS |

Example:

```
SOFTWARE: uv
START_TIME: 2024-02-14 15:30:00
END_TIME: 2024-02-14 15:31:45
SOURCE: https://docs.astral.sh/uv/getting-started/installation/ (official, recommended)
VERSION: 0.5.24 (latest stable)
ACTIONS: curl -LsSf https://astral.sh/uv/install.sh | sh (official install script)
CONFIG: Added to PATH via ~/.bashrc
TEMP: None
VERIFY: uv --version returned 0.5.24
RESULT: SUCCESS
```

## List of Reference Files

See `reference/*`.
