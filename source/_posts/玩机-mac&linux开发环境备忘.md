---
title: MAC/Linux开发环境配置备忘
top: false
cover: false
toc: true
mathjax: false
date: 2020-04-14 16:02:21
tags: ["Mac"]
password:
summary:
categories: ["玩机"]
---

系统自带 shell：
/bin/bash
/bin/csh
/bin/dash
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
设置 zsh 为默认 shell
安装 oh-my-zsh

- linux 命令行中的 curl & wget [对比](https://www.cnblogs.com/lsdb/p/7171779.html)
- sh 命令

# 更改 shell

- `brew install zsh zsh-completions` / `sudo apt install zsh`
- `[sudo] chsh -s $(which zsh)` / `chsh -s /usr/local/bin/zsh`

# 安装 oh-my-zsh For ZSH shell

[官方引导](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH)
[推荐教程](https://juejin.im/post/6844903939121348616)

## 通过 github

- `git clone git://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh`
- `cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc`

## 通过 curl/wget

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`
`sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

# 安装 3 个插件

安装

- [autojump](https://github.com/wting/autojump),实现目录间快速跳转，想去哪个目录直接 j + 目录名，不用在频繁的 cd 了

zsh-autosuggestions(命令进一步补全)
zsh-syntax-highlighting(日常用的命令会高亮显示，命令错误显示红色)
[embedding](https://asciinema.org/docs/embedding)
四个插件
`brew install autojump`
`git clone git://github.com/zsh-users/zsh-autosuggestions`
`$ZSH_CUSTOM/plugins/zsh-autosuggestions`
`git clone git://github.com/zsh-users/zsh-syntax-highlighting`
`$ZSH_CUSTOM/plugins/zsh-syntax-highlighting`
并在`.zshrc`中添加

```JS
plugins=(
  autojump
  zsh-autosuggestions
  zsh-syntax-highlighting
)
```

> 注：切换成 zsh 后，需要把 bash shell `.bashrc` 的 PATH 配置项 合并到 `.zshrc`

# 安装 nvm

# 安装 yarn
