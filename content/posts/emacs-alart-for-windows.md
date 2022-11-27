+++
title = "Use alart in emacs for windows"
author = ["jain.y"]
description = "Use growl for emacs alert"
date = 2015-10-29T00:00:00+08:00
keywords = ["alart", [",", "emacs"], [",", "windows"], [",", "growlnotify"], [",", "growlforwindows"]]
tags = ["emacs", "growlnotify"]
draft = false
+++

## alart 是个什么 {#alart-是个什么}

使用 alart 可以实现在 emacs 中通过弹窗的方式给用户发消息。
alart 支持 Mac, 在 Mac 下可以使用 Growl for Mac。
alart 默认是支持 Growl for Mac, 无需其他设置，安装好 Growl 和 growlnotify 后直接可以使用。

请从 EmacsWIki 找答案：[EmacsWiki: Alert](http://www.emacswiki.org/emacs/Alert)

alert Github：  [jwiegley/alert](https://github.com/jwiegley/alert)


## Mac 下的 Growl，Growl for Mac {#mac-下的-growl-growl-for-mac}

要在 Mac 下使用 alart，先要安装 `growl` 和 `growlnotify` 。


### AppStore 中给出的说明是这样的 {#appstore-中给出的说明是这样的}

Growl 是 Mac 通知系统的终极解决方案。这是获知各种应用的情况或通知的最简单快速的方式,无论您是否在电脑前。

有天我们在 Adium 上交流,同时在处理 iPhoto 上的照片。因为谈话的内容比较重要,我们不得不在收到每条消息时都切换应用来查看。无论是否与这次谈话相关,我们都得切换到 Adium 才能知道具体内容。这就是 Growl 出现的原因,来回切换很让人崩溃。实时的通知系统能让你快速了解你需要知道的一切。Growl 友好的通知和 Rollup 窗口,让你不会漏掉所有消息,无论你是否暂时离开了你的 Mac。

之前从未有如此个性化并高效的通知系统。你能以多种方式接收通知,包括显示在屏幕上,通过电子邮件,甚至可以让电脑读出来。另外,如果你懂得一点 Web 技术(CSS/XHTML/Javascript),你就可以创建你自己想要的 Growl 通知样式。

列出一些很棒的特性。

-   自定义通知。选择 Growl 的样式,尝试体验一下。
-   在你离开的时候 Growl 还是努力工作。 然后回到 Mac,在 Rollup 里看看你离开的时候都发生了什么。
-   易用的应用栏,定制你想收到和拒绝的消息。
-   使用 Speech 样式来听通知,对于视力略差的使用者很有帮助。
-   实用的通知历史,帮助你纵观所发生的全部。
-   Growl 自带许多不同的样式。比如 Nano 样式显示地都很小,而 Music Video 则都是大字。
-   用 Web 技术发明你自己的 Growl 样式,用你的品味定制 Growl。
-   从 Cocoa,AppleScript,网络都能给 Growl 发送通知。
-   网络支持。两个或多个 Mac 之间就能互相转发通知。
-   通过 Prowl,甚至可以在 iPhone 和 iPad 上收到通知。


### 如何获得 Growl for Mac {#如何获得-growl-for-mac}

`Grow fro Mac` 的官方主页：[Growl](http://growl.info/)

`Grow` 官方兼容 OS X 10.7 或更高版本的是收费的，不过有一个 [Growl Fork](http://www.appinn.com/growl-fork/) ，是在 Growl 最后开放的源代码版本(1.22 版)基础上改进的，修复了原来版本在 Mac OS X 10.7 Lion 崩溃的问题，依然免费提供。

`Growl Fork` 可以从这里下载：[Growl Fork – 免费的消息通知系统 Mac](http://www.appinn.com/growl-fork/)

下载后直接安装便可使用，在安装完 `Growl` 后，还要安装 `growlnotify` ，在 Extras 下。顾名思义， `growlnotify` 是给 `growl` 发通知的。

安装好了 `growl` 和 `growlnotify` 后，在 emacs（需要装 alert）中

```emacs-lisp
(alert "Hello growl")
```

屏幕上就会出现以当前 buffer 名为标题的弹窗："Hello growl"。


### alert 如何工作 {#alert-如何工作}

那么 Emacs 的 alert 是如何工作的呢。


#### alert 中是如何使用 growl 的 {#alert-中是如何使用-growl-的}

```emacs-list
(defun alert-growl-notify (info)
  (if alert-growl-command
      (let ((args
             (list "--appIcon"  "Emacs"
                   "--name"     "Emacs"
                   "--title"    (alert-encode-string (plist-get info :title))
                   "--message"  (alert-encode-string (plist-get info :message))
                   "--priority" (number-to-string
                                 (cdr (assq (plist-get info :severity)
                                            alert-growl-priorities))))))
        (if (and (plist-get info :persistent)
                 (not (plist-get info :never-persist)))
            (nconc args (list "--sticky")))
        (apply #'call-process alert-growl-command nil nil nil args))
    (alert-message-notify info)))
```


#### 参数都是什么？ {#参数都是什么}

最后的调用命令是

```text
growlnotify --appIcon Emacs --name Emacs --title emacs-alart-for-windows.org --message test --priority 0
```


## Windows 下的 Growl，Growl for windows {#windows-下的-growl-growl-for-windows}

下载地址：<http://www.growlforwindows.com/gfw/>

下载安装在控制台下测试都没有问题，可是通过 Alert 怎么也掉不出。后来看下来 alert-growl-notify，穿的参数都是 Growl For Mac 的参数格式，
Windows 下水土不服。


### Growl for Windows {#growl-for-windows}


#### 使用帮助：[Growl for Windows](http://www.growlforwindows.com/gfw/help/growlnotify.aspx) {#使用帮助-growl-for-windows}

```text
NAME

     growlnotify -- Send a Growl notification to a local or remote host

SYNOPSIS

     growlnotify [/t:title] [/id:id] [/s:sticky] [/p:priority] [/i:icon] [/c:coalescingid]
            [/a:application] [/ai:appicon] [/r:types] [/n:type]
            [/cu:callbackurl]
            [/host:host] [/port:port]
            [/pass:password] [/enc:algorithm] [/hash:algorithm]
            [/silent:suppressoutput]
            messagetext
```

虽然有了 `Growl for Windows` ，但是给它们传的参数形式不一样

|                 | Mac                   | Windows     |
|-----------------|-----------------------|-------------|
| Applicaton Icon | --appIcon AppIcon     | /ai:appicon |
| Name            | --name Name           | /n:type     |
| Title           | --title Title         | /t:title    |
| Message         | --message Messagetext | messagetext |
| Priority        | --priority Priority   | /p:priority |


#### 解决思路 {#解决思路}

一个是再写一个 (defun alert-growlwin-notify (info)) ，只看得懂一点 emacs lisp，写的难度太大，所以就用了第二个方法，加了一层 Wrapper，拿到参数的值再传给 `growlnotify` 。
