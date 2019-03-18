---
title: Make Tmux Pretty
date: 2019-03-18 16:28:14
tags:
description: This is the article to beautify the powerful terminal tool -- Tmux
---

## Make Tmux Efficient

在看本教程之前，已经默认装好了tmux组件。由于初始的tmux组件很丑，所以首先需要的就是对tmux的美化工作。关于美化我所使用的是gpakosz的库https://github.com/gpakosz/.tmux

### Installation

Requirements:

* tmux **>= 2.1** running inside Linux, Mac, O
* outside of tmux, `$TERM` must be set to `xterm-256color`

To install, run the following from your terminal: (you may want to backup your existing `~/.tmux.conf` first)

```
$ cd
$ git clone https://github.com/gpakosz/.tmux.git
$ ln -s -f .tmux/.tmux.conf
$ cp .tmux/.tmux.conf.local .
```

Then proceed to [customize](https://github.com/gpakosz/.tmux#enabling-the-powerline-look) your `~/.tmux.conf.local` copy.



### Customization

对于自定义操作，其中在`~/.tmux.conf.local`中有较多的configuration可以进行修改操作，这里可以参照github中的指示。

对于个人自定义的操作，有以下几个是比较推崇的。

* **Send prefix**:  把prefix的 `ctrl+b` 变为了 `ctrl+a` ，因为这样按起来方便些。同时，长按`prefix` 再按方向键可以做到调整pane大小的作用
* **Use Alt-arrow keys to switch panes** : 不用按prefix，直接用alt+箭头在pane之间switch。
* **Shift arrow to switch windows**: 不用按prefix，直接用shift+箭头在window之间switch。
* **Mouse mode**: 开启鼠标模式。用鼠标就能切换window，pane，还能调整pane的大小。
* **Set easier window split keys**: 这一部分是用来更方便切分pane的。`prefix` + `v` 代表竖着切，`prefix` + `h` 代表横着切。比起默认的切割方法不仅直观而且方便。主要是默认的方式实在是难受。