---
layout:     post
title:      "Youtube-dl食用技巧"
date:       2018-010-11
author:     "xx"
header-img: "img/13.jpg"
tags:
    - 技术
---
# 安装

基于Ubuntu 16.04

```
apt-get install python
pip install --upgrade youtube-dl
apt-get install ffmpeg
```

# Youtube

由于Youtube的视频在大于等于1080P的时候，视频和音频是分开的，所以下载要先下载视频和音频然后合并起来
直接下载是这样的,没有声音

<video src="https://delta.whoa.top/video/Stellaris_video_only.mp4" width="100%" height="100%" controls="controls" type="video/mp4">
</video>

这里以`https://www.youtube.com/watch?v=zW3YB2ptGws`中的视频为例

用`youtube-dl -F [url]`来获取视频的格式码

`youtube-dl -F https://www.youtube.com/watch?v=zW3YB2ptGws`

![](https://delta.whoa.top/post/ffmpeg/3.jpg)

可见 137是1080P的视频，140和157都是音频

然后用`youtube-dl -f [format code] [url]`来下载视频

所以用`youtube-dl -f 137+140 https://www.youtube.com/watch?v=zW3YB2ptGws`下载，合并得到MP4文件

如果是 137+251 合并到的是MKV文件

![](https://delta.whoa.top/post/ffmpeg/4.jpg)

下载字幕和视频

`youtube-dl --write-sub [url]`

`youtube-dl --write-sub https://www.youtube.com/watch?v=zW3YB2ptGws`

![](https://delta.whoa.top/post/ffmpeg/8.jpg)
得到英文字幕

列出可以下载的字幕

`youtube-dl --list-subs https://www.youtube.com/watch?v=zW3YB2ptGws`

![](https://delta.whoa.top/post/ffmpeg/7.jpg)

下载中文字幕

`youtube-dl --write-sub --sub-lang zh-CN https://www.youtube.com/watch?v=zW3YB2ptGws`

# 下载Bilibili视频

以这个为例

https://www.bilibili.com/video/av25383955

【底特律：变人】震撼人心的BGM是怎样炼成的（作曲团队采访）

`youtube-dl -F https://www.bilibili.com/video/av25383955`

![](https://delta.whoa.top/post/ffmpeg/9.jpg)

code为`1`

`youtube-dl -f 1 https://www.bilibili.com/video/av25383955`

下载完成以后记得重命名为1-2.flv，便于下一步操作。

先转换为ts格式；

`for i in {1..2};do ffmpeg -i $i.flv -c copy -bsf:v h264_mp4toannexb -f mpegts $i.ts;done`

进行合并即可

`ffmpeg -i "concat:1.ts|2.ts" -c copy -bsf:a aac_adtstoasc -movflags +faststart 01.mp4`

最后结果
<video src="https://delta.whoa.top/video/Detroit_BGM.mp4" width="100%" height="100%" controls="controls" type="video/mp4">
</video>

<video src="https://delta.whoa.top/video/Stellaris.mp4" width="100%" height="100%" controls="controls" type="video/mp4">
</video>

参考
https://www.jianshu.com/p/076b59bd577d
https://blog.csdn.net/qq_28484355/article/details/79181245