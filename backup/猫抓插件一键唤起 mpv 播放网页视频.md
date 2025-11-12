### 背景

最近几天有点闲下来了，突然想起两年前追番的《凡人修仙传》，当时进度刚到星海飞驰，现在已经 160 多话了，正好拿出来看看。出于顺手，我直接在小网站看的（age[.]tv），但是又不满足于过于垃圾的网页播放和画质，所以我都是把视频链接手动复制到 terminal 然后开 mpv 套 [Anime4K](https://github.com/bloc97/Anime4K) 看的。

获取网页视频链接用的是 [猫抓](https://github.com/xifangczy/cat-catch) 插件，非常好用，已使用许久。

就这么操作了好多话之后我后知后觉意识到，对啊猫抓肯定有用外部程序打开媒体的功能，为什么不用上？

### 准备工作

- [猫抓](https://github.com/xifangczy/cat-catch) 插件
- [mpv](https://github.com/mpv-player/mpv)，或者任何其他支持播放 http(s) 资源的播放器
- [URLProtocol](https://github.com/xifangczy/URLProtocol) - 猫抓作者做的快捷注册 URL Protocol 的小工具

### 步骤

**1. 注册 `mpv` URL Protocol**

协议名：`mpv`，目标程序：`C:\Users\xxx\scoop\apps\mpv\current\mpv.exe`，点击添加。

**2. 测试 URL Protocol**

浏览器地址栏中访问 `mpv:`，能成功唤起 mpv。

**3. 配置猫抓调用外部程序**

猫抓设置页面，找到「调用程序」一栏，打开「启动」，参数设置：`mpv:"${url}"`。

此处冒号前面的 `mpv` 跟前面步骤注册的协议名应当匹配。

修改会自动保存。

**4. 试一下！**

回到小网站，打开猫抓，此时在资源后面会多出一个「调用」按钮，点击即可唤起 mpv 播放本条媒体。

因为 mpv 会在成功解析链接后才出现窗口，所以点击之后可能需要等一下。如果长时间没出现窗口大概是出错了，手动复制资源链接在 terminal 调一次 mpv 看报错吧。

### 小结

感谢作者提供的全套解决方案，实在太简单了。而且整个流程都 100% 符合直觉，我都很惊讶我竟然手动开 mpv 看了好多话才反应过来...