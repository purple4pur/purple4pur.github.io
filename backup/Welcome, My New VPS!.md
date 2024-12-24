### General info

- Vendor: Tencent Cloud
- 地域和可用区: 东京 | 东京一区
- 套餐类型: 入门型
- 实例规格:
    - CPU - 2核
    - 内存 - 2GB
    - 系统盘 - SSD云硬盘 50GB
    - 流量包 - 1024GB/月（峰值带宽：30Mbps）
- Price: 99CNY for 1Yr

### VPS all-in-one test

<https://github.com/spiritLHLS/ecs>

<details><summary>Results</summary>

```
--------------------- A Bench Script By spiritlhl ----------------------
                   测评频道: https://t.me/vps_reviews
VPS融合怪版本：2024.11.08
Shell项目地址：https://github.com/spiritLHLS/ecs
Go项目地址：https://github.com/oneclickvirt/ecs
---------------------基础信息查询--感谢所有开源项目---------------------
 CPU 型号          : Intel(R) Xeon(R) Gold 6146 CPU @ 3.20GHz
 CPU 核心数        : 2
 CPU 频率          : 3192.500 MHz
 CPU 缓存          : L1: 64.00 KB / L2: 8.00 MB / L3: 27.50 MB
 AES-NI指令集      : ✔ Enabled
 VM-x/AMD-V支持    : ❌ Disabled
 内存              : 300.93 MiB / 1.80 GiB
 Swap              : [ no swap partition or swap file detected ]
 硬盘空间          : 2.24 GiB / 49.10 GiB
 启动盘路径        : /dev/vda1
 系统在线时间      : 0 days, 4 hour 51 min
 负载              : 0.50, 0.19, 0.08
 系统              : Debian GNU/Linux 12 (bookworm) (x86_64)
 架构              : x86_64 (64 Bit)
 内核              : 6.1.0-26-amd64
 TCP加速方式       : cubic
 虚拟化架构        : KVM
 NAT类型           : Port Restricted Cone
 IPV4 ASN          : AS132203 Tencent Building, Kejizhongyi Avenue
 IPV4 位置         : Tokyo / Tokyo / JP
----------------------CPU测试--通过sysbench测试-------------------------
 -> CPU 测试中 (Fast Mode, 1-Pass @ 5sec)
 1 线程测试(单核)得分:          1022 Scores
 2 线程测试(多核)得分:          1975 Scores
---------------------内存测试--感谢lemonbench开源-----------------------
 -> 内存测试 Test (Fast Mode, 1-Pass @ 5sec)
 单线程读测试:          22664.25 MB/s
 单线程写测试:          14543.86 MB/s
------------------磁盘dd读写测试--感谢lemonbench开源--------------------
 -> 磁盘IO测试中 (4K Block/1M Block, Direct Mode)
 测试操作               写速度                                  读速度
 100MB-4K Block         4.8 MB/s (1171 IOPS, 21.85s)            10.5 MB/s (2553 IOPS, 10.03s)
 1GB-1M Block           127 MB/s (121 IOPS, 8.23s)              267 MB/s (254 IOPS, 3.93s)
---------------------磁盘fio读写测试--感谢yabs开源----------------------
Block Size | 4k            (IOPS) | 64k           (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 65.65 MB/s   (16.4k) | 286.13 MB/s   (4.4k)
Write      | 65.79 MB/s   (16.4k) | 287.64 MB/s   (4.4k)
Total      | 131.45 MB/s  (32.8k) | 573.77 MB/s   (8.9k)
           |                      |
Block Size | 512k          (IOPS) | 1m            (IOPS)
  ------   | ---            ----  | ----           ----
Read       | 265.10 MB/s    (517) | 258.28 MB/s    (252)
Write      | 279.18 MB/s    (545) | 275.48 MB/s    (269)
Total      | 544.29 MB/s   (1.0k) | 533.76 MB/s    (521)
------------流媒体解锁--基于oneclickvirt/CommonMediaTests开源-----------
以下测试的解锁地区是准确的，但是不是完整解锁的判断可能有误，这方面仅作参考使用
----------------Netflix-----------------
[IPV4]
您的出口IP可以使用Netflix，但仅可看Netflix自制剧
NF所识别的IP地域信息：新加坡
[IPV6]
您的网络可能没有正常配置IPv6，或者没有IPv6网络接入
----------------Youtube-----------------
[IPV4]
连接方式: Youtube Video Server
视频缓存节点地域: 日本 东京(NRT12S24)
Youtube识别地域: 日本(JP)
[IPV6]
Youtube在您的出口IP所在的国家不提供服务
---------------DisneyPlus---------------
[IPV4]
当前出口所在地区解锁DisneyPlus
区域：JP 区
[IPV6]
DisneyPlus在您的出口IP所在的国家不提供服务
解锁Netflix，Youtube，DisneyPlus上面和下面进行比较，不同之处自行判断
----------------流媒体解锁--感谢RegionRestrictionCheck开源--------------
 以下为IPV4网络测试，若无IPV4网络则无输出
============[ Multination ]============
 Dazn:                                  Yes (Region: JP)
 Disney+:                               No (IP Banned By Disney+ 1)
 Netflix:                               Originals Only
 YouTube Premium:                       Yes (Region: JP)
 Amazon Prime Video:                    Yes (Region: JP)
 TVBAnywhere+:                          Yes
 Spotify Registration:                  Yes (Region: JP)
 Instagram Licensed Audio:              No
 OneTrust Region:                       JP [Tokyo]
 iQyi Oversea Region:                   JP
 Bing Region:                           JP
 YouTube CDN:                           Tokyo
 Netflix Preferred CDN:                 Tokyo
 ChatGPT:                               No (Only Available with Web Browser)
 Google Gemini:                         Yes (Region: JPN)
 Wikipedia Editability:                 No
 Google Search CAPTCHA Free:            Yes
 Steam Currency:                        JPY
 ---Forum---
 Reddit:                                No
=======================================
 以下为IPV6网络测试，若无IPV6网络则无输出
---------------TikTok解锁--感谢lmc999的源脚本及fscarmen PR--------------
 Tiktok Region:         【JP】
-------------IP质量检测--基于oneclickvirt/securityCheck使用-------------
数据仅作参考，不代表100%准确，如果和实际情况不一致请手动查询多个数据库比对
以下为各数据库编号，输出结果后将自带数据库来源对应的编号
ipinfo数据库  [0] | scamalytics数据库 [1] | virustotal数据库   [2] | abuseipdb数据库   [3] | ip2location数据库    [4]
ip-api数据库  [5] | ipwhois数据库     [6] | ipregistry数据库   [7] | ipdata数据库      [8] | db-ip数据库          [9]
ipapiis数据库 [A] | ipapicom数据库    [B] | bigdatacloud数据库 [C] | cheervision数据库 [D] | ipqualityscore 数据库 [E]
IPV4:
安全得分:
声誉(越高越好): 0 [2]
信任得分(越高越好): 0 [8]
VPN得分(越低越好): 100 [8]
代理得分(越低越好): 100 [8]
社区投票-无害: 0 [2]
社区投票-恶意: 0 [2]
威胁得分(越低越好): 100 [8]
欺诈得分(越低越好): 0 [1] 65 [E]
滥用得分(越低越好): 0 [3]
ASN滥用得分(越低越好): 0.0031 (Low) [A]
公司滥用得分(越低越好): 0.0133 (Elevated) [A]
威胁级别: low [9 B]
黑名单记录统计:(有多少黑名单网站有记录):
无害记录数: 0 [2]  恶意记录数: 0 [2]  可疑记录数: 0 [2]  无记录数: 94 [2]
安全信息:
使用类型: hosting [0 7 9 A] business [8] hosting - high probability [C] DataCenter/WebHosting/Transit [3]
公司类型: business [A] hosting [0 7]
是否云提供商: Yes [7 D]
是否数据中心: Yes [0 1 5 6 A C] No [8]
是否移动设备: Yes [E] No [5 A C]
是否代理: Yes [E] No [0 1 4 5 6 7 8 9 A B C D]
是否VPN: Yes [0 A E] No [1 6 7 C D]
是否Tor: No [0 1 3 6 7 8 A B C D E]
是否Tor出口: No [1 7 D]
是否网络爬虫: No [9 A B E]
是否匿名: No [1 6 7 D] Yes [8]
是否攻击者: No [7 8 D]
是否滥用者: No [7 8 A C D E]
是否威胁: No [7 8 C D]
是否中继: No [0 7 8 C D]
是否Bogon: No [7 8 A C D]
是否机器人: No [E]
DNS-黑名单: 314(Total_Check) 0(Clean) 5(Blacklisted) 15(Other)
Google搜索可行性：YES
-------------邮件端口检测--基于oneclickvirt/portchecker开源-------------
Platform  SMTP  SMTPS POP3  POP3S IMAP  IMAPS
LocalPort ✔     ✔     ✔     ✔     ✔     ✔
QQ        ✘     ✔     ✔     ✘     ✔     ✘
163       ✘     ✔     ✔     ✘     ✔     ✘
Sohu      ✘     ✔     ✔     ✘     ✔     ✘
Yandex    ✘     ✔     ✔     ✘     ✔     ✘
Gmail     ✘     ✔     ✘     ✘     ✘     ✘
Outlook   ✘     ✘     ✔     ✘     ✔     ✘
Office365 ✘     ✘     ✔     ✘     ✔     ✘
Yahoo     ✘     ✔     ✘     ✘     ✘     ✘
MailCOM   ✘     ✔     ✔     ✘     ✔     ✘
MailRU    ✘     ✔     ✘     ✘     ✔     ✘
AOL       ✘     ✔     ✘     ✘     ✘     ✘
GMX       ✘     ✘     ✔     ✘     ✔     ✘
Sina      ✘     ✘     ✔     ✘     ✔     ✘
----------------三网回程--基于oneclickvirt/backtrace开源----------------
北京电信 219.141.140.10  检测不到回程路由节点的IP地址
北京联通 202.106.195.68  联通4837   [普通线路]
北京移动 221.179.155.161 移动CMI    [普通线路]
上海电信 202.96.209.133  检测不到回程路由节点的IP地址
上海联通 210.22.97.1     联通4837   [普通线路]
上海移动 211.136.112.200 检测不到回程路由节点的IP地址
广州电信 58.60.188.222   检测不到回程路由节点的IP地址
广州联通 210.21.196.6    联通4837   [普通线路]
广州移动 120.196.165.24  移动CMI    [普通线路]
成都电信 61.139.2.69     检测不到回程路由节点的IP地址
成都联通 119.6.6.6       联通4837   [普通线路]
成都移动 211.137.96.205  移动CMI    [普通线路]
准确线路自行查看详细路由，本测试结果仅作参考
同一目标地址多个线路时，可能检测已越过汇聚层，除了第一个线路外，后续信息可能无效
---------------------回程路由--感谢fscarmen开源及PR---------------------
依次测试电信/联通/移动经过的地区及线路，核心程序来自ipip.net或nexttrace，请知悉!
广州电信 58.60.188.222
0.56 ms         * RFC1918
51.64 ms        * RFC1918
1.04 ms         AS2516 [KDDI] 日本 东京都 东京 kddi.com
1.30 ms         AS2516 [KDDI] 日本 东京都 东京 kddi.com
2.13 ms         AS2516 [KDDI] 日本 东京都 东京 kddi.com
8.56 ms         AS2516 [KDDI] 日本 大阪府 大阪市 kddi.com
9.97 ms         AS2516 [KDDI] 日本 大阪府 大阪市 kddi.com
7.90 ms         AS2516 [KDDI] 日本 大阪府 大阪 kddi.com
* ms    AS2516 [APNIC-AP] 中国 上海 KDDI-CT-Peer kddi.com
* ms    AS4134 [CHINANET-BB] 中国 广东 广州 www.chinatelecom.com.cn
* ms    AS4134 中国 广东 深圳 福田区 www.chinatelecom.com.cn 电信
广州联通 210.21.196.6

广州移动 120.196.165.24

--------------------自动更新测速节点列表--本脚本原创--------------------
位置             上传速度        下载速度        延迟     丢包率
Speedtest.net    29.19 Mbps      224.88 Mbps     1.28     0.0%
日本东京         29.75 Mbps      126.26 Mbps     1.10     NULL
中国香港         29.79 Mbps      226.47 Mbps     50.12    0.0%
联通WuXi         26.51 Mbps      20.04 Mbps      213.37   16.8%
电信Suzhou5G     29.04 Mbps      6.23 Mbps       276.79   NULL
移动Beijing      30.67 Mbps      252.70 Mbps     99.08    NULL
------------------------------------------------------------------------
 总共花费      : 7 分 24 秒
 时间          : Sun Nov 17 20:58:43 CST 2024
------------------------------------------------------------------------
  短链:
    https://paste.spiritlhl.net/code/kqiPsk.txt
    http://hpaste.spiritlhl.net/code/kqiPsk.txt
```

</details>

### Setup actions

- Enlarge ssh timeout setting:

```sh
(root) vim /etc/ssh/sshd_config
```

```diff
-#ClientAliveInterval 0
-#ClientAliveCountMax 3
+ClientAliveInterval 120
+ClientAliveCountMax 30
```

```sh
(root) systemctl restart ssh
```

- Add user and grant sudo:

```sh
(root) adduser purple4pur # interactive
(root) groupadd njupt
(root) usermod -aG njupt purple4pur
getent group njupt
(root) EDITOR=vim visudo
```

```diff
+purple4pur ALL=(ALL) ALL
```

- Install tools:

[[Installing CLI Tools I Use on Debian/Linux]](https://blog.quitw.org/post/Installing%20CLI%20Tools%20I%20Use%20on%20Debian-Linux.html)

- Initialize dotfiles:

```sh
bin/chezmoi init --apply https://github.com/purple4pur/dotfiles.git
vim .bashrc
```

```diff
+export PATH=$($HOME/custom_scripts/prepend-path $HOME/bin $HOME/opt/nvim-linux64/bin)
+export EDITOR=nvim
+alias v='nvim'
+source ~/.config/.bash_setup
```

```sh
source .bashrc
```

- Good to go!