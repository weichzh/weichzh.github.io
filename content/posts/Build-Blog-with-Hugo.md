+++
title = "用 Hugo 搭建博客 | 用 org-mode 写作"
author = ["Weichzh"]
date = 2023-03-15T20:23:00+08:00
lastmod = 2023-03-15T21:40:26+08:00
tags = [":技术:"]
draft = false
+++

<div class="ox-hugo-toc toc">

<div class="heading">&#30446;&#24405;</div>

- [导言](#导言)
- [目标](#目标)
- [准备阶段](#准备阶段)
- [创建网站](#创建网站)
- [设置](#设置)
    - [Hugo](#hugo)
    - [Org-mode](#org-mode)
- [写作](#写作)

</div>
<!--endtoc-->


## 导言 {#导言}

介绍如何使用 Hugo 搭建博客的文章多如牛毛，即便是专门讲 Orgmode 与 Hugo 结合的文章也是数不胜数。本文章旨在为将自己搭建博客的过程记录下来，一则避免未来走弯路，二则也是为其他有类似需求的用户踩雷。


## 目标 {#目标}

希望这次就能做到，写作方便，网站功能齐全，简洁雅观。这其实意味着要更多的自定义，但幸好有珠玉在前，虽然还是走了不少弯路。


## 准备阶段 {#准备阶段}

安装 Hugo 就不用多说了。Emacs 的配置才是重点。

我使用的是 [Doom Emacs](https://github.com/doomemacs/doomemacs) 配置，需要在 init.el 文件里找到 `org` ，然后改成：

```emacs-lisp
(org +brain +dragdrop +roam2 +pretty +hugo +gnuplot +jupyter +pandoc +pomodoro)
```

其中 `+hugo` 这个是必须加的，其余的，加不加由你，我反正是加了。

然后要在 config.el 文件中，把 `org-directory` 等变量设置好。具体可以看看一些介绍
Doom Emacs 和 org-mode 的文章。以及为了解决一些中文相关的问题，看 [Emacs 相关中文问题以及解决方案](https://github.com/hick/emacs-chinese) 来解决这些问题。我以后可能会写一篇关于 org-mode 的文章，因为现在文章多少都有些不太合适，有些是复杂了，有些是过于简单了。

在把诸如 Org-capture 等设置好后，还能坚持下来的，那么恭喜，接下来就是 **享受** Emacs
和 Orgmode 的强大了。


## 创建网站 {#创建网站}

首先在终端里输入：

```shell
hugo new site <Your blog name> -f yml
```

之所以用 yml 作为博客的配置文件格式，是因为我要用 [PaperMod](https://github.com/adityatelange/hugo-PaperMod/) 作为博客的主题。下面开始导入主题。

```shell
cd blog # 进入你的博客文件夹
git init # 我将使用 git submodule 作为导入主题的方式，所以需要先将博客转为 git 仓库
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
git submodule update --init --recursive
```

如果之后要升级主题，则使用：

```shell
git submodule update --remote --merge
```

然后使用 `hugo server -D` 打开网站预览。


## 设置 {#设置}


### Hugo {#hugo}

PaperMod 官方就有一个配置样例。但这个样例不是太好。可以看看 [这篇文章](https://www.sulvblog.cn/posts/blog/build_hugo/) 。

Google Analytics 的 `SiteVerificationTag` 要去 Google Analytics 的 **管理** 创建 \*
媒体资源\* 后，再在 **媒体资源** 下面的 **数据流** ，把网站点进去，就可以看到 **衡量
ID** 了。这个 ID 就是我们需要的。如果发现点击进去没反应，可能是 Adguard 之类的拦截软件给拦截了。

关于 favicon ，推荐一个网站 [favicon.io](https://favicon.io/) 。

另外，像 tags 和 categories 这样的内置的页面，名字没有办法通过设置改成中文的，除非改代码。这个看看上面提到的样例网站的[ 源码](https://github.com/xyming108/sulv-hugo-papermod) 就知道了。


### Org-mode {#org-mode}

看 [ox-hugo](https://ox-hugo.scripter.co/) 的文档。可以说，我碰到的问题这个文档都能解决。

创建 front matter 当然不用自己手打，有 Yasnippet 这个神器。我的 snippet 如下：

```nil
# key: hugo
# name: hugo
# --
#+HUGO_BASE_DIR: ../
#+TITLE: $1
#+DATE: `(format-time-string "[%Y-%m-%d %a %H:%M]")`
#+HUGO_AUTO_SET_LASTMOD: t
#+HUGO_TAGS: $2
#+HUGO_DRAFT: false
#+AUTHOR: Weichzh
#+TOC: headlines 3

$0
```


## 写作 {#写作}

在 blog 里面新建一个 org 文件夹，专门放 org 文件（ Hugo 当然可以解析 org 文件，但效果显然没有 markdown 好）。

写作时，首先在 org 文件夹里新建一个 org 文件，然后敲出 `hugo` 补全，用 `<tab>`
来在各个属性间跳转。

写好之后，保存，在终端里用 `hugo` 命令生成网页。