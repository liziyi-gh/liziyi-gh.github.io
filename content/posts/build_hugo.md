---
title: "搭建本站"
date: 2022-04-02T23:59:48+08:00
draft: false
---
<!-- 搭建 hugo 的过程记录 -->
<!-- <\!--more-\-> -->

# 缘起
近来越来越有写作欲望，所以想配一个漂亮的博客。
# 过程
## 框架选择
流行的博客框架当中原生支持 ```org-mode``` 的似乎只有 ```hugo```，于是乎就它了。
主要原因是偷懒不想在 ```emacs``` 里配 ```markdown``` 的环境了。
尽管这篇博文最后还是用了 ```markdown``` 写……

## 主题选择
试了 ```ananke``` ,```diary```, ```whiteplain``` 都不是很满意，甚至想自己写一个了。
现在先用着 ```diary``` ，之后再做一些定制吧，前端的东西确实是一头雾水。
## 搭建过程
```
sudo pacman -S hugo
hugo new site blog
git submodule add https://github.com/AmazingRise/hugo-theme-diary.git themes/diary
hugo server -D
```
默认的内容配置似乎是在 ```content/posts``` 下面。
# 结语
尽力多写一些。
