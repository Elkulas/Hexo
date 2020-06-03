---
title: sigprocmask()中的set和oldset
date: 2020-06-03 15:59:23
tags: 
description: CMU15613 学习signals block时候着重记录一下sigprocmask()函数中的一些变量与要点
---

# sigprocmask()中的set和oldset

在学习的过程中，发现man page以及书本上都对oldset解释的不是很清楚，所以在这边自己稍稍记录一下。其中主要是参照了[stackoverflow](https://stackoverflow.com/questions/25261/set-and-oldset-in-sigprocmask)上的这位老哥，看完之后自己也就懂了很多。



## 函数原型

```
int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
```

其中how表示了怎么做，也就是这个函数的功能：

​       SIG_BLOCK

​              The set of blocked signals is the union of the current set and the set argument.将set中的signal添加到block vector中



​       SIG_UNBLOCK

​              The signals in set are removed from the current set of blocked signals.  It is permissible to attempt to unblock a signal which is not blocked. 将set中的signal从block中删除



​       SIG_SETMASK

​              The set of blocked signals is set to the argument set. 也就是从blocked vector中的signal复制给set：blocked = set



在这里主要讲讲set和oldset



## 例子

有以下代码，假定在原来的blocking list为 ```{SIGSEGV, SIGSUSP}```

```
sigset_t x;
sigemptyset (&x);
sigaddset(&x, SIGUSR1);
sigprocmask(SIG_BLOCK, &x, NULL)
```

那么新的blocking list就会变成```{SIGSEGV, SIGSUSP, SIGUSR1}```

如果继续调用如下函数：

```
sigprocmask(SIG_UNBLOCK, &x, NULL)
```

那么新的blocking list就会变成```{SIGSEGV, SIGSUSP}```

如果现在再继续调用如下函数：

```
sigprocmask(SIG_SETMASK, &x, NULL)
```

那么新的blocking list就会变成```{SIGUSR1}```



```oldset```变量主要是存储blocking list中的内容。不妨有如下的定义

```
sigset_t y;
```

然后调用以下函数

```
sigprocmask(SIG_BLOCK, &x, &y)
```

那么可以获得

```
y == {SIGSEGV, SIGSUSP}
```

继续调用以下函数

```
sigprocmask(SIG_UNBLOCK, &x, &y)
```

可以获得

```
y == {SIGSEGV, SIGSUSP, SIGUSR1}
```

继续调用以下函数

```
sigprocmask(SIG_SET, &x, &y)
```

可以获得

```
y == {SIGSEGV, SIGSUSP}
```

所以可以看出来，```oldset```中的变量和global中的blocking list是对应的。理解了这一点这个函数就比较好理解了。