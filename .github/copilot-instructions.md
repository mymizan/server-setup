# Copilot / AI Agent Instructions for server-setup

Summary
- This repository contains small, focused Bash scripts for provisioning Debian/Ubuntu servers.
- Primary scripts: [update.sh](update.sh#L1-L3), [essential-install.sh](essential-install.sh#L1-L2), [nvm.sh](nvm.sh#L1-L2), [docker.sh](docker.sh#L1-L1).

Big picture
- Purpose: provide manual automation steps to bring a fresh server to a usable state (system updates, common tools, Node version manager, Docker setup placeholder).
- Architecture: no services or compiled components — a set of independent scripts executed in sequence by a human operator or a simple orchestration wrapper.

Key developer workflows
- Run scripts locally (recommended order):
  1. `sudo bash update.sh` (runs `apt update` && `apt upgrade -y`) — see [update.sh](update.sh#L1-L3).
  2. `sudo bash essential-install.sh` (installs packages such as `vim`) — see [essential-install.sh](essential-install.sh#L1-L2).
  3. `bash nvm.sh` (pipes remote nvm install script to bash) — network connectivity required; runs as the target user, not root — see [nvm.sh](nvm.sh#L1-L2).
  4. `bash docker.sh` (currently a placeholder; add Docker install commands here) — see [docker.sh](docker.sh#L1-L1).
- Make scripts executable when used interactively: `chmod +x update.sh essential-install.sh nvm.sh docker.sh` then `sudo ./update.sh` etc.
- Important: scripts expect a Debian-based environment (use of `apt`). They are not designed for macOS or non-Debian Linux unless adapted.

Project-specific conventions and patterns
- Scripts are simple, single-purpose Bash files with a shebang (`#!/bin/bash`). Prefer preserving this style when adding new scripts.
- Keep scripts idempotent where practical (e.g., `apt install -y pkg || true` or wrap checks) — current scripts are minimal and assume fresh runs.
- Network calls: `nvm.sh` downloads and executes a remote installer. Note this side-effect pattern when auditing security.

Integration points & external dependencies
- External network access: `nvm.sh` uses `curl` to fetch from GitHub; provisioning requires outbound internet.
- Package manager: `apt` commands assume Debian/Ubuntu environment and root privileges.
- No CI, tests, or container images are present. Any automation added should document expected OS and privilege requirements.

What an AI agent should do when contributing
- Be conservative: when modifying scripts that run package managers or remote installers, add a short comment explaining why and any OS assumptions.
- If adding new scripts, follow existing naming and shebang conventions and include a short usage example at the top.
- When suggesting changes that affect security (e.g., replacing a `curl | bash`), propose safer alternatives and include migration notes.

Examples (copyable)
- Make scripts executable and run the canonical sequence:

```bash
chmod +x update.sh essential-install.sh nvm.sh docker.sh
sudo ./update.sh && sudo ./essential-install.sh
./nvm.sh
```

Next steps / questions
- If you want, I can: (a) expand `docker.sh` with an idempotent Docker install for Ubuntu, (b) make scripts idempotent, or (c) add a wrapper `bootstrap.sh` that runs the canonical sequence and logs actions. Tell me which to implement.
