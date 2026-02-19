# OpenClaw

> **WARNING**: This file is incomplete. All content is unverified and subject to change.

## Official Documentation
- Official site (look for installation/trouble shooting guides from here): https://docs.openclaw.ai/install/

## Prerequisites

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo >> /home/admin/.bashrc
echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"' >> /home/admin/.bashrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv bash)"
sudo apt-get install build-essential
brew install gcc
git clone https://github.com/zenggyu/max && mv max .openclaw
```

## User Preferences
- Installation method preference: Use official install script
- Version preference: latest version (stable or not)

## Definition of Functional
Running `openclaw --version` returns exit code 0 and outputs version string.

## Post-Installation Configuration

```bash
echo export PATH="/home/admin/.npm-global/bin:$PATH" >> ~/.bashrc
openclaw onboard --install-daemon
openclaw plugins install @soimy/dingtalk
openclaw gateway restart
openclaw configure --section channels
```

- During onboarding:
    - skills: github, clawhub
    - hooks: command-logger, session-memory

## Notes

The following notes are intended for human, agents should ignore these notes.

On server A:

```bash
sudo apt update
sudo apt install git podman
cd ~
git clone https://github.com/openclaw/
cd openclaw
sudo ./setup-podman.sh
podman image save localhost/openclaw:local | gzip > /tmp/openclaw.tar.gz
```

On server B (note: admin is the name of the user on server B to run the container, and it must have the same uid as the user in the container, i.e., 1000, as implied by the Dockerfile； run the folllowing commands as this user):

```bash
sudo apt update
sudo apt install git podman # then make sure /etc/subuid and /etc/subgid have an entry for the admin user, e.g., admin:100000:65536
scp <user>@<serverA>:/tmp/openclaw.tar.gz /tmp/
podman image load < /tmp/openclaw.tar.gz
cd ~
git clone https://github.com/openclaw/openclaw/
git clone https://github.com/zenggyu/max
export OPENCLAW_CONFIG_DIR=/home/admin/max
export OPENCLAW_PODMAN_USER=admin
openclaw/scripts/run-openclaw-podman.sh launch setup # not required
openclaw/scripts/run-openclaw-podman.sh launch # required
```
