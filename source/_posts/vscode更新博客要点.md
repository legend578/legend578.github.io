---
title: vscode更新博客要点
date: 2025-04-15 20:23:40
categories: # 分类
	- 博客初始化  # 只能由一个
tags: # 标签
	- 初始化  # 能有多个
	- 博客  # 一个标签一行
---

## hexo博客更新
需要新增一篇博客时直接在blog文件夹下hexo new “文件名”

---

## 推送信息
当在vscode更新时需要每次从本地提交到远程，再重新启动才会生效
修改完内容后：
1. git add -A(添加到缓冲区)
2. git commit -m "此次修改的描述信息"
3. git push(推送到远程;如果出现fatal: unable to access 'https://github.com/legend578/legend578.github.io.git/': Failed to connect to 127.0.0.1 port 7897 after 2107 ms: Could not connect to server则进行开启clash代理)
hexo d（使得网站重新生效）
---

## 怎么将博客放入到标签和分类中
在每篇博客的顶部，进行设置categories和tags，想要将该博客归属到哪，就在下面编写对应文字
<img src="/img/vscode1.png" width="auto" height="auto">

## 怎么在博客中插入图片
使用<img src="/img/vscode1.png" width="auto" height="auto">，使用img标签即可，src填写图片路径，可以是外部图片，也可以是本机图片