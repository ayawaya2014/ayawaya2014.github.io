---
title: 在Opensuse上用docker搭建Jellyfin视频流媒体服务
lang: zh-CN
tags:
  - 學習
  - Learn
  - Study
  - Scientific Research
categories:
  - other
abbrlink: 20facba3
date: 2025-05-28 07:21:27
---

Jellyfin是一个免费的媒体管理系统，让您能够自主管理和串流您的媒体。它是专有的Emby和Plex的替代方案，通过多个应用程序从专用服务器向用户设备提供服务。Jellyfin源自Emby的3.5.2版本，并被移植到.NET Core框架，以支持完全的跨平台使用。没有任何附加条件，没有高级许可证或功能，也没有隐秘的意图：只有一个希望构建更好事物并共同努力实现它的团队。

<!--more-->

Jellyfin Server运行在Linux，Windows，macOS等多平台。在Linux上安装有多种方法，首先根据发行版进行判断，Debian和Redhat系都有直接安装包，没有直接安装方式的发行版可以自行编译，或者使用docker：
```
docker pull jellyfin/jellyfin:latest  # or docker pull ghcr.io/jellyfin/jellyfin:latest
mkdir -p /srv/jellyfin/{config,cache}
docker run -d -v /srv/jellyfin/config:/config -v /srv/jellyfin/cache:/cache -v /media:/media --net=host jellyfin/jellyfin:latest
```

docker pull jellyfin/jellyfin
Run Jellyfin in Docker. Example commands store data in /srv/jellyfin and assume your media is stored under /media.