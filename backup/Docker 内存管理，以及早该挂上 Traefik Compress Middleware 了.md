最近时不时给我的小鸡 VPS 加容器，本就捉襟见肘的 2GB 内存一不注意就直接爆满。而幽默腾讯云在遇到 VPS 内存 100% 的时候会直接死机，症状是完全连不上服务器，服务全部下线，腾讯云网页监控显示磁盘繁忙 100%，同时丢失 CPU/MEM 监控数据，这种情况下我的唯一解决办法是强制重启。

死机下线遇到好几次我决定解决这个问题。第一件事是检查每个容器的内存占用，然后限制合理的内存上限。

```
# 显示每个容器的 CPU/MEM 等数据
docker stats
```

```
# compose.yml 设定内存上限
services:
  SERVICE:
    deploy:
      resources:
        limits:
          memory: 100M
```

顺带一提，当容器超过设定上限之后会直接 OOM (Out-of-Memory) 退出，所以搭配上 `restart: unless-stopped` 一起用，OOM 后自动重启。这里的关键是想办法降低容器内应用的内存，以及配置合理的内存上限。

自然引出了第二个需求，我想监控 VPS 和 docker 相关的系统资源占用情况，这样的工具还是很多的，这里我用了 [cAdvisor](https://github.com/google/cadvisor)。默认配置很简单，但我很快遇到了一个大问题：cAdvisor 本身就很吃内存和带宽。

占内存的问题参考 [cadvisor#2523](https://github.com/google/cadvisor/issues/2523) 能降低到我可以接受的程度，但 WebUI 出公网流量非常大确实也很影响体验。

最终我在 [DOZZLE 的 FAQ](https://dozzle.dev/guide/faq#disabling-compression-in-traefik) 里得到启发，我只要在 Traefik 里挂上 compress middleware 是不是就能快速解决这个问题？

```
# 定义一个 compress middleware，命名为 compressor
labels:
  traefik.http.middlewares.compressor.compress: true
  traefik.http.middlewares.compressor.compress.excludedContentTypes: "text/event-stream"
```

```
# 服务挂上 compressor
labels:
  traefik.http.routers.SERVICE.middlewares: "compressor"

# 如果需要串联多个 middlewares
labels:
  traefik.http.routers.SERVICE.middlewares: "basic_auth,compressor"
```

结果证明非常有效，公网流量从 3-4 MB/s 骤降至 200-300 KB/s。这么好用的中间件我怎么没有早点用上？于是火速把我所有 https 公网服务都挂上了，无感又有效，不压缩传数据简直猪鼻（对的我是🐽）。

对了，Traefik 文档好像没写怎么串联中间件，只有 Chain 有个例子，我只好从那里抄来试试。要是文档真没写那也挺🐽的。