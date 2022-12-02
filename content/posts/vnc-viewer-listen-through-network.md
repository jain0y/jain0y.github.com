+++
title = "使用 vnc 穿透内网远程控制"
description = "使用 vnc viewer 的监听功能连接内网机器远程桌面"
date = 2016-07-06T00:00:00+08:00
keywords = ["vnc", [",", "listen"], [",", "内网穿透"]]
tags = ["vnc", [",", "远程"], [",", "内网"]]
draft = false
+++

## 什么是 vnc？ {#什么是-vnc}

-   VNC (Virtual Network Computer)是虚拟网络计算机的缩写。它 是一款优秀的远程控制工具软件，由著名的 AT&amp;T 的欧洲研究实验室开发的。
    VNC 是在基于 UNIX 和 Linux 操作系统的免费的开源软件，远程控制能力强大，高效实用，其性能可以和 Windows 和 MAC 中的任何远程控制软件媲美。

    在 Linux 中，VNC 包括以下四个命令：vncserver，vncviewer，vncpasswd，和 vncconnect。

    大多数情况下用户只需要其中的两个命令：vncserver 和 vncviewer。

-   关于 vnc 的服务端配置请自行搜索。
    -   Windows 版:
        -   [Download TightVNC](http://www.tightvnc.com/download.php)
        -   [RealVNC | Download VNC for Windows](https://www.realvnc.com/download/vnc/windows/)


## 穿越内网远程控制 {#穿越内网远程控制}

-   服务端（vnc server）在内网，客户端（vnc viewer）需要有公网 IP。原理是由服务端反向朝着客户端（需要监听端口）发送连接请求。

-   vnc viewer 监听
    -   下载:  [RealVNC | Download VNC Viewer](https://www.realvnc.com/download/viewer/)
    -   windows 版启动监听

        在快捷方式中加入 `-listen` 参数，启动后会最小化到任务栏图标。

    -   linux 启动监听
        ```text
              vncviewer -listen
        ```

-   vnc server 端请求
    ```text
        vncconfig -display :1 -connect 监听端公网-IP:端口（默认 5500 端口)
    ```


## 遇到的问题 {#遇到的问题}

-   如果客户端也在内网的话，也是无法直接连接的（不能内网 to 内网，只能内网 to 公网）。必须把内网映射到外网。有以下两种方法：
    -   设置 DMZ 主机：主机地址设置为客户端机器 IP
    -   端口映射：把外网端口映射到客户端机器的 5500 端口，比如：公网 8800 -&gt; 客户机 5500，服务器端连接方式如下：
        ```text
              vncconfig -display :1 -connect 公网-IP:8800
        ```
