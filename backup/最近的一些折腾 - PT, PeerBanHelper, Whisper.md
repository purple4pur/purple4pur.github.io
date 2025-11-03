正好七天前入坑了 PT，感谢群友 Moroshima 和 Picpo 的介绍和邀请。今天也正好通过了此站的新手考核，总的来说没什么难度。

玩 PT 在技术上好像没什么新东西，因为我之前也挂了几年 BT，基本上无痛迁移了，0 学习成本。得益于活跃的 PT 社区，绝大部分种子都有相当可观的下载速度，轻松吃满带宽，这种爽感是 BT 很难带来的。

折腾了 PT 就顺势又折腾了 [PeerBanHelper](https://github.com/PBH-BTN/PeerBanHelper)。这个项目我倒是知道挺久了，但一直没部署过，因为当初看文档感觉有点麻烦。现在再读文档感觉又不一样了，有 docker 一切都方便了起来。在本地简单测试了之后，我决定把 PBH 部署到东京的 VPS 上，确保服务时刻在线。

另外入坑 PT 的第二天我意识到可以部署复数个客户端，于是连上了位于广东的 [homelab](/post/37.html)，给它也折腾部署上了 [qbittorrent-nox](https://github.com/qbittorrent/docker-qbittorrent-nox)，它也成为了 PT 24/7 online client 的光荣一员。这么一番部署之后，网络的拓扑就有点幽默：PT 客户端分别在上海和广东，而它们都连着位于东京的 PBH。

前两天周末我看着硬盘里下好了还没看的种子，又萌生了用 LLM 生成字幕的想法，于是开始折腾 Whisper。在尝试了一两个 cli 封装之后，目前选用的是 [whisper-ctranslate2](https://github.com/Softcatala/whisper-ctranslate2)，还在尝试各种参数中。Whisper 我跑在 homelab 上，老东西放着也是放着，不如出来拉练一下。