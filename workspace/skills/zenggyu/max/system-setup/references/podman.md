# Podman

> **WARNING**: This file is incomplete. All content is unverified and subject to change.

## Official Documentation
- Installation guide: https://podman.io/docs/installation
- Configuration guide: 
- Troubleshooting: https://podman.io/docs

## User Preferences
- Installation method preference: Use official repository (not distro package)
- Version preference: Latest stable
- Prefer rootless mode for personal machines

## Definition of Functional
Running `podman --version` returns exit code 0 and outputs version string.

## Post-Installation Configuration
- Find a registry mirror from mainland China (trusted: `pulswr.cn-south-1.myhuaweicloud.com`, `docker.m.daocloud.io` and `docker.1ms.run`), make sure it's accessible (try pulling a small image; ask user for direction if none is accessible), and add it to `/etc/containers/registries.conf`.
