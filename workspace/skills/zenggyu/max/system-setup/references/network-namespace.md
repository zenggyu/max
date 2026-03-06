# Network Namespace

## Overview

This reference file takes Yunmai VPN as an example to demonstrate how to set up a Linux network namespace and run Yunmai as well as other processes inside the namespace, so that all network traffic of these processes will go through the VPN, while other processes outside the namespace will not be affected.

## Prerequisites

Yunmai installed on the Linux system, and is properly configured to function as a VPN client (can successfully connect to the VPN server and access intranet resources).

## Create A Network Namespace Setup Service

Create a script `/usr/local/bin/yunmai-ns-setup.sh` with the following content to set up the network namespace and routing rules:

```bash
#!/bin/bash

# 0. 清理可能已存在的配置
ip netns del yunmai_ns 2>/dev/null
ip link del veth_yunmai0 2>/dev/null
# 1. 创建网络命名空间
ip netns add yunmai_ns
# 2. 创建 veth 设备对
ip link add veth_yunmai0 type veth peer name veth_yunmai1
# 3. 将 veth_yunmai1 移到命名空间
ip link set veth_yunmai1 netns yunmai_ns
# 4. 配置 IP 地址
ip addr add 10.200.1.1/24 dev veth_yunmai0
ip netns exec yunmai_ns ip addr add 10.200.1.2/24 dev veth_yunmai1
# 5. 启用接口
ip link set veth_yunmai0 up
ip netns exec yunmai_ns ip link set veth_yunmai1 up
ip netns exec yunmai_ns ip link set lo up
# 6. 在命名空间内配置默认路由
ip netns exec yunmai_ns ip route add default via 10.200.1.1
# 7. 获取默认网络接口
IFACE=$(ip route get 8.8.8.8 | awk '{print $5; exit}')
# 8. 配置 NAT 和转发规则
iptables -t nat -A POSTROUTING -s 10.200.1.0/24 -o "$IFACE" -j MASQUERADE
iptables -A FORWARD -i veth_yunmai0 -o "$IFACE" -j ACCEPT
iptables -A FORWARD -i "$IFACE" -o veth_yunmai0 -m state --state RELATED,ESTABLISHED -j ACCEPT
# 9. 启用 IP 转发
sysctl -w net.ipv4.ip_forward=1
```

Make the script executable:

```bash
sudo chmod +x /usr/local/bin/yunmai-ns-setup.sh
```

Create a systemd service file `/etc/systemd/system/yunmai-ns.service` to run the above script at startup:

```
[Unit]
Description=Setup yunmai network namespace
After=network-online.target
Wants=network-online.target
[Service]
Type=oneshot
ExecStart=/usr/local/bin/yunmai-ns-setup.sh
RemainAfterExit=yes
[Install]
WantedBy=multi-user.target
```

Enable and start the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable yunmai-ns.service
sudo systemctl start yunmai-ns.service
```

## Run VPN Services in the Namespace

There are two Yunmai services that need to run inside the `yunmai_ns` namespace: `yunmai-daemon.service` and `yunmai-updater.service` ^[There are two other Yunmai services: `yunmai-screenshot.service`, `yunmai-cross.service`; but they do not affect VPN connection and are not required to run in the namespace.]. We can modify their systemd service files to add the namespace configuration, so that they will automatically run inside the namespace when started.

Use `sudo systemctl edit <service-name>` to create an override file for `yunmai-daemon.service` and `yunmai-updater.service` separately, and add the following content to the override files:

```
[Unit]
After=yunmai-ns.service
Requires=yunmai-ns.service

[Service]
NetworkNamespacePath=/run/netns/yunmai_ns
```

Then reload systemd and restart the services:

```bash
sudo systemctl daemon-reload
sudo systemctl restart yunmai-daemon.service
sudo systemctl restart yunmai-updater.service
```

## Run Other Applications in the Namespace

Take google-chrome as an example, we can create a `.desktop` file (e.g., `~/.local/share/applications/yunmai-chrome.desktop`) to run it in the namespace.

First, use `mkdir -p ~/.config/google-chrome-yunmai` to create a separate user data directory for this instance of chrome, to avoid conflicts with the normal chrome instance.

Then create the `.desktop` file with the following content:

```
[Desktop Entry]
Version=1.0
Name=Chrome-yunmai
GenericName=Web Browser in yunmai namespace
Comment=Access the Internet in yunmai network namespace
Exec=/bin/bash -c 'sudo /usr/sbin/ip netns exec yunmai_ns runuser -u "$USER" -- /usr/bin/google-chrome --class=Chrome-yunmai --user-data-dir="$HOME/.config/google-chrome-yunmai %U"'
StartupNotify=true
Terminal=false
Icon=/usr/share/icons/breeze/categories/32/applications-internet.svg
StartupWMClass=Chrome-yunmai
Type=Application
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml+xml;x-scheme-handler/http;x-scheme-handler/https;
```

Note that `/usr/sbin/ip netns exec` requires root privilege, so we need to configure sudoers to allow the user to run this command without password. Add the following line to the sudoers file (using `sudo visudo`):

```
<user-name> ALL=(ALL) NOPASSWD: /usr/sbin/ip netns exec *
```

After the setup, when you launch the "Chrome-yunmai" application, it will run inside the same `yunmai_ns` namespace as the Yunmai VPN services, and all its network traffic will go through the Yunmai VPN.

## Verify the Setup

You can use the following command to check whether the processes are running in the correct namespace:

```bash
for pid in $(pgrep -f 'yunmai-daemon|yunmai-updater|Chrome-yunmai'); do
  ns_name=$(sudo ip netns identify $pid 2>/dev/null)
  cmd=$(ps -p $pid -o cmd= 2>/dev/null | cut -c1-60)
  echo "PID $pid [$ns_name]: $cmd"
done
```

If the setup is correct, you should see that `yunmai-daemon`, `yunmai-updater`, and the "Chrome-yunmai" process are all running in the `yunmai_ns` namespace, and you can use the "Chrome-yunmai" application to access intranet resources through the Yunmai VPN.
