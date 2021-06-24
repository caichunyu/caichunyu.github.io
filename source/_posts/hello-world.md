---
title: Hello World
# top_img: ./img/index.png
categories: 
- 记录
tags:
- hexo
- Gitalk
---
一般学习新的语言，第一行代码就是hello world，所以我博客的第一篇文章名称就是它了，本文主要讲一下在安装过程中遇到的小问题，包括hexo-theme-next在本地显示正常，部署之后无css样式；使用Gitalk评论无法正常显示；还有主页的默认图片路径问题。

## hexo-theme-next css样式不显示
本地正常显示，GitHub page 部署后css样式不显示这个是GitHub库设置的路径问题，解决方法为，在hexo配置文件_config.yml内，找到 url 这个配置项，里面设置url和root跟目录的位置。比如我的站点路径是 https://xxxxxx.github.io/ 那么，将root设置为 / 红色圈中是官方的注释
![hexo-theme-next css样式不显示](/img/2021/hexo-theme-next_css样式不显示.png)

## 默认图片路径
主页，文章页默认图片路径设置为绝对路径，在文章中引入图片有下面三种办法：
    绝对引用: 在 /source 下放图片文件夹
    相对引用: 使用 post_asset_folder:true 建立文章同名文件夹
    CDN 或者图床引用
注意方法一的话无法在markdown中预览，因为路径不一样。

## Gitalk评论无法正常显示
我遇到的问题是显示，未找到相关的Issues评论，请联系***初始化创建，然后登录的话就会进入我的博客主页，然后发现是回调地址的问题，详细参见：[hexo next 主题配置 gitalk 评论后无法初始化创建 issue](https://github.com/gitalk/gitalk/issues/115)。
