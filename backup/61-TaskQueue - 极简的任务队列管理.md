先放链接：<https://github.com/purple4pur/taskqueue>

为什么有这样的需求？自从玩 PT 之后我经常需要在 Homelab 上用 ffmpeg 压一遍视频，这样才方便自己能通过小水管远程播放，然而 Homelab 机器也超级老古董了，压视频本身很慢，若是同时开好多个 ffmpeg 则效率非常低，因此我希望有这样一个工具能帮我排队执行任务。

先前大致在网上找了一下类似功能的开源工具，但没有合适的，或是过于笨重，或是常年缺乏维护。好在最终我意外看到了堪比恩师的一篇博客 - [A Job Queue in 20 Lines of Bash](https://maximerobeyns.com/fragments/job_queue) ，文中的 tasks.txt 和 runner.sh 非常简单明了地实现了我需要的功能，不多不少。

与此同时我也注意到了一些不太方便使用的地方，比如 tasks.txt 相当依赖手动编辑。因此我借助 DeepSeek 帮我完成了一套管理脚本。其实它原先写得很复杂，我手动删减和整理了不少，才得到现在这样足够精简的实现。

自己写工具来解决自己的需求真的超级爽。Enjoy :)