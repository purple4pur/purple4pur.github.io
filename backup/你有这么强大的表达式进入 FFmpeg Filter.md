最近在用 FFmpeg 批量转视频编码到 AV1，牺牲画质来换取低码率文件，以满足我走超小水管还能流畅播放 Homelab 上的视频的需求。

为了进一步降低码率，我想对视频画面的像素量也砍一大刀。原视频为 1080p 的我的小水管都受不了，更别提还有一些 4k 的文件了。既然牺牲画质那就贯彻到底，经过一番探索，最终的 FFmpeg 参数如下：

[完整脚本 (gist.github.com)](https://gist.github.com/purple4pur/45e5e52cf8f8a9717113e732c79ab9d8)

```sh
ffmpeg -hide_banner -nostdin \
    -i "$file" \
    -c:v libsvtav1 -preset 9 \
    -vf "scale=min(iw\\,if(gte(a\\,1)\\,1920\\,1080))*3/4:-2,fps=min(source_fps\\,30)" \
    -c:a aac -b:a 128k \
    "$target_file" \
    -y
```

其中主要发力的参数是多线程友好的 AV1 编码器 libsvtav1，以及大砍像素量的 vf (video filter)。在 filter 表达式里可以灵活使用 FFmpeg 自身支持的无敌强大的函数，直接可以写出复杂的条件判断，这个真是我始料未及的。

简单拆解一下：

```c
// Filter #1: scale - 缩放画面
width = min (
            in_width,          // 原视频宽度
            横版视频 ?         // 宽高比 (a) > 1 即为横版视频
                1920 : 1080
        ) * 3/4 ;              // 宽度像素砍至 3/4
height = 根据 width 自动适配 ; // -2 确保适配的结果为偶数

// Filter #2: fps - 视频帧数
fps = min (
          source_fps,          // 原视频帧数
          30
      );
```

为什么要给 width 套一层 min()，因为如果碰到原视频为 4k，直接砍到 3/4 结果还比 1080p 大不少，于是干脆把超过 1080p 的素材当作 1080p 来处理。

我本来以为要实现这种功能需要自己写脚本处理判断逻辑的，没想到 if else 和大小判断全都自带了，省了大量额外工作。

最后贴文档：

- [FFmpeg Filters Documentation](https://ffmpeg.org/ffmpeg-filters.html)
- [FFmpeg Expression Evaluation](https://ffmpeg.org/ffmpeg-utils.html#Expression-Evaluation)
- [Lake1059/FFmpegFreeUI](https://github.com/Lake1059/FFmpegFreeUI?tab=readme-ov-file#%E8%A7%86%E9%A2%91%E7%BC%96%E7%A0%81%E5%99%A8) - FFmpeg 支持编码器的实用介绍
- [SVT-AV1 - What Presets Do](https://gitlab.com/AOMediaCodec/SVT-AV1/-/blob/master/Docs/CommonQuestions.md#what-presets-do) - libsvtav1 各个 preset 对应的 feature