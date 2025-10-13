### 前言

这台 Mac mini 已在我家服役多年，当初家里买了它之后，还不太懂电脑的我不知怎么摸索着给装上了 Windows，这可能还真得感谢 bootcamp 做得如此小白友好。作为当时家里性能最好的电脑，又碰上好不容易可以放开玩电脑的我，它陪伴了我注册 Steam，帮助我通关了人生中的第一部 3A 单机《虐杀原形 2》。直到 2017 年我装了家里的台式，这台 Mac mini 也就逐渐转为备用机，再到慢慢退役。

最近一两年家里客厅的几台 PC 又时不时出现各种小问题，所以这台 Mac 仍然在作为备用机服役。最近几个月终于稳定下来的，客厅唯一在用的 PC 变成了我的一台老笔记本电脑。我这次国庆回家的时候，Mac 已经完全闲置了。正好我也知道它无论是启动 Windows 11 还是 macOS 都卡顿无比（我感觉是机械硬盘的问题），于是我就萌生了把它装上 Linux 的想法，不要桌面环境，以后就当成个 home lab 来用。

### 前期准备

先简单搜了下，这样的操作是有先例的，这样我才继续下一步准备。另外开工前以防万一，还是分别启动了两个系统检查有没有要备份的数据。

- [Debain 13 ISO (netinst)](https://www.debian.org/distrib/netinst)

我选择的是最小安装镜像，不带桌面环境。

- U 盘一个

家里库存的 U 盘全都太古董了，我前一天在京东随便买了个 64GB 的，今天刚好到。

- [Ventoy](https://github.com/ventoy/Ventoy)

最好用的启动盘制作器没有之一。用 Ventoy 格式化好 U 盘之后，把 ISO 放进 exFAT 分区就完事了。

### 安装

插上 U 盘，开机后按住 Alt/Cmd 直到看到启动选项。选择启动插入的 U 盘（带 EFI 字样的），进入 Ventoy 界面，选择已经放入的 Debian ISO，注意这里要选择用 grub2 mode，如果用 normal mode 会报 "Not a secure boot platform 14"。

加载 Debian ISO 之后，我选择了不要 GUI 的安装模式，这样安装过程就全部是 TUI，其实效果也相当好了。一步步执行下去，有两个地方要留意下：

选择安装分区 - 其中有一个选择是需不需要 setup LVM，反正我查了下是在物理盘上又加了一层抽象，可以让分区跨物理盘，说是对个人用户来说没啥用。

设置 DHCP - 安装过程我是一直插着网线的，但在这一步自动设置失败了，还好不影响后续的安装，可以选择跳过这一步。但因为后续安装都缺少网络连接，所以只会安装 ISO 自带的最小系统和软件，在安装完成后我配置的一些环节可能也跟这里设置失败有关。

总之整个安装还是相当顺利的，几乎没有什么停顿 Debian 就装好了，下一步即可进入系统。

### 登录 Debian 并配置网络和 ssh

安装过程中除了 root 还引导创建了一个 user，但因为还在最早期的系统配置，我就先用 root 登录了。

第一个问题自然是先连上有线网。首先 `ip l` 可以看到有线网卡是可以识别到的，网卡名是 `enp3s0f0`。根据 [NetworkConfiguration](https://wiki.debian.org/NetworkConfiguration)，需要手动编辑 `/etc/network/interfaces`，还只能用 nano，对的这会儿系统里还没有 vim 哈哈：

```
auto enp3s0f0
allow-hotplug enp3s0f0
iface enp3s0f0 inet dhcp
```

这样就把网卡配置为 ipv4 DHCP 了。保存文件后 `systemctl restart networking`，可能会花一两分钟，然后 `routel` 就能看到通过 DHCP 获取的 ipv4 地址了，此时已连接上有线网。

第二个问题是 apt 竟然没有自带源，打开 `/etc/apt/sources.list` 后发现里面只有一个已经注释掉的 cdrom 源，并没有任何官方在线源，猜测可能跟安装过程中没有网络有关。根据 [Debian archive basics](https://www.debian.org/doc/manuals/debian-reference/ch02.en.html#_debian_archive_basics) 填入官方源后就可以正常使用 apt 了：

```
deb http://deb.debian.org/debian/ trixie main non-free-firmware contrib non-free
deb-src http://deb.debian.org/debian/ trixie main non-free-firmware contrib non-free

deb http://security.debian.org/debian-security trixie-security main non-free-firmware contrib non-free
deb-src http://security.debian.org/debian-security trixie-security main non-free-firmware contrib non-free
```

这里还有个大陆网络问题，我在家的话其他设备就直接走我手机上的代理。Debian 这边配一下 `http_proxy` 大部分软件就能认：

```bash
export http_proxy=http://192.168.50.254:2334
export https_proxy=http://192.168.50.254:2334
```

下一步是要解决 Wifi 问题，因为网线我是临时从旁边 PC 上拔过来的，不是长久之计。`ip l` 找不到无线网卡，通过 `lspci -k` 可以看到无线网卡型号是 Broadcom BCM4360。根据 [wl](https://wiki.debian.org/wl) 安装驱动：

```bash
apt-get install linux-image-$(uname -r|sed 's,[^-]*-[^-]*-,,') linux-headers-$(uname -r|sed 's,[^-]*-[^-]*-,,') broadcom-sta-dkms
dpkg-reconfigure broadcom-sta-dkms
modprobe -r b44 b43 b43legacy ssb brcmsmac bcma
modprobe wl
```

安装完成后就能识别到无线网卡了。我在这里卡了很久，尝试过网上说的很多方法，最后我发现关键点在于 `broadcom-sta-dkms` 已经代替有些旧方法提到的 `bcmwl-kernel-source` 等包了，以及最重要的，一定要运行 `dpkg-reconfigure broadcom-sta-dkms` 来编译生成需要的 `wl` 驱动。这一步在参考网页上列为 Optional，但在我这个情况里其实是 Required。

识别到无线网卡后用 Network manager 连接 Wifi，首先需要 `apt install network-manager`，然后参考 [nmcli-examples](https://manpages.debian.org/trixie/network-manager/nmcli-examples.7.en.html)，连接过程非常傻瓜式。

下一步开 ssh server 解放物理机的屏幕和键盘，对的怎么你连 ssh server 都没有预装。根据 [SSH](https://wiki.debian.org/SSH)，先安装 server `apt install openssh-server`，然后根据需要改 `/etc/ssh/sshd_config`：

```
PermitRootLogin yes
ClientAliveInterval 120
ClientAliveCountMax 30
```

最后重启 sshd `systemctl restart sshd`，这样就配置好了，测试没问题即可拔掉键盘鼠标和 HDMI，只剩一根电源线。

到此最基本的配置就结束了，已经解决了最为关键的网络和 ssh，下一步是作为用户安装一些我常用的软件。

### 安装软件

以前我整理过一篇个人必备的软件和安装方法，[链接在此](/post/14.html)，后来我发现很多开源软件我从 github release 装比从官方源装要方便和快得多，于是这类软件基本都转用 [bin](https://github.com/marcosnils/bin) 了。不得不说 bin 确实是个好工具，我以前写了个 python 版的 Scoop，刚实现最基本的安装卸载功能就发现 bin 已经完美覆盖我的使用场景了。

以前在服务器上我都是把 bin 装在 `$HOME/bin` 里，这次我突然意识到其实完全可以装在 `/usr/local/bin` 里让所有人都能用，所以我先用 root 装了这些软件：

```
Path                          Version  URL                            Status
/usr/local/bin/bin            v0.23.1  github.com/marcosnils/bin      OK
/usr/local/bin/nvim.appimage  v0.11.4  github.com/neovim/neovim       OK
/usr/local/bin/chezmoi        v2.65.2  github.com/twpayne/chezmoi     OK
/usr/local/bin/starship       v1.23.0  github.com/starship/starship   OK
/usr/local/bin/fd             v10.3.0  github.com/sharkdp/fd          OK
/usr/local/bin/rg             14.1.1   github.com/BurntSushi/ripgrep  OK
```

这里有个小问题是装好的二进制的权限都是 `rwxr--r--`，为了让所有人都能用还得 `chmod +x /usr/local/bin/*`，感觉非常不优雅。

docker 还是得用 apt 安装，参考 [Install using the apt repository](https://docs.docker.com/engine/install/debian/#install-using-the-repository)。注意装完之后可以把自己的 username 加到 docker group：`usermod -aG docker purple4pur`。dockerd 的代理需要单独设置，参考 [Daemon proxy configuration](https://docs.docker.com/engine/daemon/proxy/)。

chezmoi 用来快速同步我的 dotfiles，后续就跟设置 VPS 的部分一样了，可以参考文章 [Welcome, My New VPS!](/post/16.html)。

至此所有配置已完成！

### 后记

Mac mini 装 Debian 我计划了半天，然后执行了半天，总体来说顺利得有点出乎预料。对比安装 Windows，可能因为我选择了不装桌面环境，所以完全没有遇到很麻烦的驱动问题，真正花了时间处理的驱动其实就一个 Wifi。

后续其实我也没想好真的拿它来做什么，可能会先配好 frp 出公网吧，这样我在上海的时候也能操作了。