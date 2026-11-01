经过简单评估，我发现很容易就能自定义 Blog builder (Gmeek.py) 的行为。于是本 Blog 转向 [forked Gmeek](https://github.com/purple4pur/Gmeek)，并优化了以下方面：

1. Blog 内部链接全面使用相对路径，不同的域名下都能顺畅跳转不改变域名，例如 <https://blog.quitw.org> 和 <https://purple4pur.github.io/> 都能工作在自己的域名内。
2. 在 footer 增加 Source Code 链接，方便我自己增改 issue。
3. 文章页显示日期和标签。我一直觉得这是个缺失的功能，现在总算能显示了。
4. Blog 主页在窄视图中保持显示站名和文章日期。
5. 文章中的表格居中显示。

感谢原作者开源！Happy Hacking!
