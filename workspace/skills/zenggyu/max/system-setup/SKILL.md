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
    - No need to read irrelevant reference files.
    - If no reference is found and the change has system/user-level impact and is persistent (as opposed to local or temporary), stick with the general guidance provided by this skill, be extra careful in the following steps, and ask user proactively.
3. **Plan** — Choose installation/configuration method:
    - Check system information (CPU architecture, operating system version, etc.).
    - Choose installation/configuration method based on system information, software-specific preferences, and general guidance.
4. **Execute** — Perform the installation/configuration actions:
    - When errors or unexpected issues occur, first consult the official documentation for solution (use the URL provided in the reference file).
    - If the official URL is not provided or does not help, ask user for guidance.
    - Do not implement custom hacks or workarounds without user approval.
    - Do not trust unofficial sources or unverified solutions.
5. **Verify** — Confirm installation/configuration is successful:
    - Follow verification steps in the reference file.
    - If reference file does not exist or does not provide verification steps, perform basic checks to ensure software is functional.
    - If the verification step produces test files or results in changes only intended for verification, clean them up afterwards.
6. **Log and report** — Record all operations and summarised results, and put them in a report under a temporary directory (e.g., `/tmp` on Linux, `%TEMP%` on Windows).

Repeat steps 2-6 for each installation/configuration task.

## General Preferences

### Source of Truth

In order of priority (from high to low; when in conflict, follow the higher priority source):

- User instruction.
- Software-specific reference files: `references/*.md`.
- General guidance: `SKILL.md`.
- Official sources ("official" means directly provided or explicitly endorsed by user or software vendor; watch out for impersonation)

Never trust anything except the sources listed above, unless user explicitly approves. For example, information from search engines, forums, or social media should not be trusted without approval. Do not blindly follow links that claim to be official.

Examples of official sources:

- URLs specified in user prompt or `references/*.md`.
- Well known vendor websites  (`https://cran.r-project.org/`, `https://www.python.org/`, etc.) and repositories (`docker.io`, `https://cloud.r-project.org/bin/`, `https://github.com/openclaw/openclaw`, etc.).
- It's okay to use mirror sites provided by authoritative organizations (e.g., university/big company owned mirror sites like `https://mirrors.tuna.tsinghua.edu.cn/CRAN`, or `https://mirrors.aliyun.com/pypi/`), but only when the primary source is unavailable or too slow; ask user if unsure about whether an organization should be considered "authoritative".

### Be Appropriately Conservative

Choose options that are up-to-date, robust, and have minimal requirements/impact:

- When choosing software versions, prefer the latest stable version over the latest unstable or old stable ones.
- Prefer user-level installation and configuration (e.g., under `~/`, or use virtual environments) over system-level installation and configuration.
- Prefer modern package managers that does a better job on dependency management, isolation, and simplifies installation (e.g., `uv` over `pip`, `bun` over `npm`).
- Prefer solutions that present lower risk (e.g., `podman` rootless container over `docker` root container).
- Prefer solutions that are more widely adopted and have better community support (e.g., `apt` over `snap` on Ubuntu).
- Prefer existing tools and workflows over introducing new ones (e.g., `apt` over `flatpak` on Ubuntu), unless there is a good reason to do otherwise (e.g., if a software is available both from `uv` and `apt`, prefer `uv` because it offers better isolation and dependency management).
- Prefer containerized solutions for services like databases, web servers, and other complex applications (exception: simple services or client-side tools like `psql`).
- General preference order of package managers: language-specific (`uv`, `bun`, etc.) > user-level (`homebrew`, `chocolatey`, etc.) > system (`apt`, `yum`, etc.) > universal (`snap`, `flatpak`, etc.).
- Unless user explicitly requests or it is clear that the software to be installed is project-specific, consistently install the software in a conventional user-level global location (for example, on Linux: `~/.uv-global` for `uv`, `~/.bun-global` for `bun`, `~/.np-global` for `npm`, `~/.linuxbrew` for `homebrew`, and `~/.local` for other single-file executables).
- Create backups before deleting or modifying existing files.
- When uncertain, ask for user guidance.

### Be Clean

Keep persistent changes minimal:

- Never install anything beyond required, unless explicitly approved by the user.
- Revert temporary changes (environment variables, configurations, processes, etc.) for verification after use.
- Temporary files should be created in `/tmp/` on Linux or `%TEMP%` on Windows and deleted after use.

### Be Transparent

Create a report for each software installation task:

- Report location:
   - Linux: `/tmp/system-setup-<yyyymmddHHMMSS>.log`.
   - Windows: `%TEMP%\system-setup-<yyyymmddHHMMSS>.log`.
- This report should be created at the beginning of the task, and should record each command and its standard output/error during the installation process.
- Report template (repeat for each command):
    ```
    "${TIMESTAMP}" # ISO-8601, e.g. 2026-03-13T10:00:00Z
    "${COMMAND}"
    "${STDOUT|STDERR}"
    ```
- Also include problems encountered, choices made, and the reasoning during the installation process.
- If there were any temporary changes that could not be reverted, document them.
- If any outdated information/practices from the reference file were discovered (by comparing with the latest official sources), document it and inform user.
- Inform user about the report after the task completes or aborts.
