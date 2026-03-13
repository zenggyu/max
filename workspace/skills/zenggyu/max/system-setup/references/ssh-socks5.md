# SSH Socks5

## Overview

This reference file shows how to create a systemd service for a persistent ssh socks5 proxy.

## Prerequisites

- Key-based authentication enabled for a remote ssh server.
- Host specification defined in `~/.ssh/config` (assuming `cloudvm` as the host name).
- Port `1080` is not occupied by existing services.

## Systemd Unit File

Create a **user level** unit file `~/.config/systemd/user/ssh-socks5.service`, and put the following content in it:

```
[Unit]
Description=Persistent SSH SOCKS5 Proxy
After=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/ssh -N -D 1080 cloudvm \
    -o ServerAliveInterval=10 \
    -o ServerAliveCountMax=3 \
    -o ExitOnForwardFailure=yes \
    -o TCPKeepAlive=yes
Restart=always
RestartSec=5

[Install]
WantedBy=default.target
```

Note the reason to setup a user level service instead of a system level service is that the host specification for `cloudvm` (i.e., `~/.ssh/config`) is defined in the user's home directory, so system level service would not see it unless using absolute path (which is inconvenient).

After creating the unit file, run the following commands to start and examine the status of the service；

```bash
systemctl --user daemon-reload
systemctl --user enable ssh-socks5.service
systemctl --user start ssh-socks5.service
systemctl --user status ssh-socks5.service
```

After the setup, the socks5 proxy should be available through `socks5://127.0.0.1:1080`.
