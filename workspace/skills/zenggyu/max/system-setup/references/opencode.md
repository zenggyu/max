# OpenCode

> **anomalyco/opencode** - The open source coding agent.
> - Repo: https://github.com/anomalyco/opencode
> - Website: https://opencode.ai

## Official Documentation
- Installation guide: https://opencode.ai/install (or https://github.com/anomalyco/opencode/releases)
- Documentation: https://opencode.ai/docs
- Troubleshooting: https://github.com/anomalyco/opencode/issues

## User Preferences
- Installation method preference: Use official install script from https://opencode.ai/install
- Version preference: Latest stable

## Installation Methods

### Official Install Script (Recommended)
```bash
curl -fsSL https://opencode.ai/install | bash
```

With custom install directory:
```bash
OPENCODE_INSTALL_DIR=/usr/local/bin curl -fsSL https://opencode.ai/install | bash
XDG_BIN_DIR=$HOME/.local/bin curl -fsSL https://opencode.ai/install | bash
```

### Package Managers

| Platform | Command |
|----------|---------|
| npm/bun/pnpm/yarn | `npm i -g opencode-ai@latest` |
| Windows (Scoop) | `scoop install opencode` |
| Windows (Chocolatey) | `choco install opencode` |
| macOS/Linux (Homebrew - recommended) | `brew install anomalyco/tap/opencode` |
| macOS/Linux (Homebrew official) | `brew install opencode` |
| Arch Linux | `sudo pacman -S opencode` or `paru -S opencode-bin` |
| Nix | `nix run nixpkgs#opencode` |

### Build from Source
```bash
go install github.com/anomalyco/opencode@latest
```

## Definition of Functional
Running `opencode --version` returns exit code 0 and outputs version string.

## Post-Installation Configuration
- Configure API keys (Claude, OpenAI, Google, etc.)
- Set up preferences in config file

## Update
```bash
opencode upgrade
```

## Notes
- OpenCode includes two built-in agents: `build` (full-access, default) and `plan` (read-only)
- Uses client/server architecture
- 100% open source, provider-agnostic (works with Claude, OpenAI, Google, local models)
- Built-in LSP support
