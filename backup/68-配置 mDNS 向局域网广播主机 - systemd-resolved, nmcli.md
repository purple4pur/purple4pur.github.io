因为想在其他内网设备连接上服务器的终端，所以有这个需求。作为网络小白，我之前的做法是在路由器侧把服务器绑定到一个固定 IP 地址，然后在其他设备上直连这个 IP 或者配置 Host。说来搞笑，我在搜索 DeepSeek CLI 的时候翻到了 OpenCode，又在 OpenCode 里翻到了 WebUI，在这篇文档里看到了 mDNS，我心想这可不就是我一直想要的功能嘛。简单了解 mDNS 之后，在 Gemini 老师的帮助下，相当顺利地完成了整个配置。

Gemini 推荐了 Avahi 但我想尽可能避免安装新的第三方软件包，于是使用了 `systemd-resolved` 搭配我正在使用的网络管理 NetworkManager。

1. 安装 `systemd-resolved`

本来以为预装了结果没有...

```sh
apt update && apt install systemd-resolved
```

安装过程会自动把系统的 DNS 服务接管给 systemd-resolved。

2. 配置 mDNS，启动 `systemd-resolved`

```sh
# Edit /etc/systemd/resolved.conf
MulticastDNS=yes

# Restart and auto-start resolved
systemctl restart systemd-resolved
systemctl enable systemd-resolved

# Verify mDNS settings
resolvectl status
# ... +mDNS ...
```

这里有一个小坑，`systemd-resolved` 自带一个默认 conf 会优先关闭 mDNS，需要排除它：

```sh
mv /usr/lib/systemd/resolved.conf.d/00-disable-mdns.conf /usr/lib/systemd/resolved.conf.d/00-disable-mdns.conf.bak
```

3. `nmcli` 配置 `systemd-resolved` 作为 DNS 后端

```sh
# Edit /etc/systemd/resolved.conf
[main]
dns=systemd-resolved

# Apply settings
systemctl restart NetworkManager
nmcli connection up "Wired connection 1"
```

以上完成 mDNS 配置，效果是向局域网广播 `主机名.local`，这个广播通常会经由路由器发送到其他设备上，其他设备就可以用这个主机名解析到本机的 IP 地址。