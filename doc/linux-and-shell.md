# Linux / Shell

Author: ika <ika@thewizardbay.cc>
Last Updated: 2023-11-10 10:30

#### Table of Contents

1.  [介绍](#orgc0fd636)
2.  [UNIX 系统与 POSIX 标准](#org185b9d3)
    1.  [UNIX 历史](#org9f85a38)
    2.  [UNIX 及类 UNIX 系统](#org62d0c55)
        1.  [流行](#org1e2f61b)
        2.  [淘汰](#orge0ed64f)
    3.  [POSIX标准](#org6525988)
    4.  [扩展阅读](#org494b224)
3.  [GNU/Linux 介绍](#orgfc635dc)
    1.  [主要系统架构](#org1385e33)
        1.  [内核与系统调用](#orgc6eeb8c)
        2.  [函式库](#orgc67c2b6)
        3.  [Shell与应用程序](#org6e6d37a)
        4.  [文件系统与架构](#orgd5682fe)
        5.  [网络架构](#orgd8aa242)
    2.  [图形界面与桌面架构](#org15d3bc7)
        1.  [显示服务器](#orgd1eea68)
        2.  [桌面环境与窗口管理器](#orgf7ebb10)
        3.  [音频系统](#orga5a6b08)
    3.  [主要发行版介绍](#orge7fa234)
        1.  [Debian](#org16349ca)
        2.  [Ubuntu](#orgf46fbb3)
        3.  [Arch](#org208ff9e)
        4.  [RHEL 及衍生发行版](#orga3cf5a3)
        5.  [其他](#org1b04c50)
    4.  [安装流程示范 (Debian 12)](#orga28e6e2)
        1.  [下载镜像](#orgf9dd843)
        2.  [刻录镜像到U盘](#orgab9f03d)
        3.  [引导](#org2a9162d)
        4.  [分区并设置挂载点](#orgaf0e794)
        5.  [其他系统设置](#orge028583)
        6.  [等待安装结束并重启](#org072a25d)
    5.  [软件与生态 (选修)](#org5a6763a)
        1.  [桌面环境与窗口管理器](#org8392863)
        2.  [终端模拟器](#orgb772e6f)
        3.  [常用专权软件的开源替代品](#org7a6c6e5)
    6.  [扩展阅读](#orge7df4ae)
4.  [终端基础](#orgc84f069)
    1.  [功能及使用方法](#org32fb321)
    2.  [快捷键](#orgf80461f)
    3.  [常用程序 (基础)](#org56a098e)
        1.  [coreutils (基础)](#org98b21aa)
        2.  [包管理器](#orgfa7d696)
        3.  [ssh](#org717437b)
        4.  [man](#org90e43ce)
        5.  [grep](#org719adf6)
        6.  [nano](#org77432fa)
        7.  [curl & wget](#org00a7b9f)
        8.  [tar & zip](#orga59a252)
    4.  [常用程序 (高阶)](#org56c797f)
        1.  [Shell](#org0911108)
        2.  [coreutils (扩展)](#org7d28577)
        3.  [sed](#orgb2e7710)
        4.  [awk](#org9fd2a9d)
    5.  [扩展 (选修)](#orgaa3c7bb)
        1.  [tmux](#org236b778)
        2.  [文件管理器](#org1d8eebb)
        3.  [Vim / Emacs](#org57805cc)
        4.  [systemd](#org96cc070)
    6.  [扩展阅读](#org332c26f)
        1.  [常用程序的现代化替代品](#org5a660ef)
5.  [Bash 编程基础](#org4b26593)
    1.  [编写, 运行, 输入与输出](#orga2e1926)
    2.  [基础控制流](#org9426ba1)
    3.  [基础数据结构](#org8e047b7)
    4.  [脚本演示](#org2f96588)
    5.  [扩展阅读](#org3061899)



<a id="orgc0fd636"></a>

# 介绍


<a id="org185b9d3"></a>

# UNIX 系统与 POSIX 标准


<a id="org9f85a38"></a>

## UNIX 历史


<a id="org62d0c55"></a>

## UNIX 及类 UNIX 系统


<a id="org1e2f61b"></a>

### 流行

1.  GNU/Linux

2.  BSD

3.  MacOS


<a id="orge0ed64f"></a>

### 淘汰

1.  Solaris

2.  HP-UX

3.  &#x2026;.

    <https://en.wikipedia.org/wiki/List_of_Unix_systems>


<a id="org6525988"></a>

## POSIX标准

<https://en.wikipedia.org/wiki/POSIX>


<a id="org494b224"></a>

## 扩展阅读


<a id="orgfc635dc"></a>

# GNU/Linux 介绍


<a id="org1385e33"></a>

## 主要系统架构


<a id="orgc6eeb8c"></a>

### 内核与系统调用


<a id="orgc67c2b6"></a>

### 函式库


<a id="org6e6d37a"></a>

### Shell与应用程序


<a id="orgd5682fe"></a>

### 文件系统与架构


<a id="orgd8aa242"></a>

### 网络架构


<a id="org15d3bc7"></a>

## 图形界面与桌面架构


<a id="orgd1eea68"></a>

### 显示服务器


<a id="orgf7ebb10"></a>

### 桌面环境与窗口管理器


<a id="orga5a6b08"></a>

### 音频系统


<a id="orge7fa234"></a>

## 主要发行版介绍


<a id="org16349ca"></a>

### Debian


<a id="orgf46fbb3"></a>

### Ubuntu


<a id="org208ff9e"></a>

### Arch


<a id="orga3cf5a3"></a>

### RHEL 及衍生发行版


<a id="org1b04c50"></a>

### 其他


<a id="orga28e6e2"></a>

## 安装流程示范 (Debian 12)


<a id="orgf9dd843"></a>

### 下载镜像


<a id="orgab9f03d"></a>

### 刻录镜像到U盘


<a id="org2a9162d"></a>

### 引导


<a id="orgaf0e794"></a>

### 分区并设置挂载点


<a id="orge028583"></a>

### 其他系统设置


<a id="org072a25d"></a>

### 等待安装结束并重启


<a id="org5a6763a"></a>

## 软件与生态 (选修)


<a id="org8392863"></a>

### 桌面环境与窗口管理器


<a id="orgb772e6f"></a>

### 终端模拟器


<a id="org7a6c6e5"></a>

### 常用专权软件的开源替代品


<a id="orge7df4ae"></a>

## 扩展阅读


<a id="orgc84f069"></a>

# 终端基础


<a id="org32fb321"></a>

## 功能及使用方法


<a id="orgf80461f"></a>

## 快捷键


<a id="org56a098e"></a>

## 常用程序 (基础)


<a id="org98b21aa"></a>

### coreutils (基础)


<a id="orgfa7d696"></a>

### 包管理器


<a id="org717437b"></a>

### ssh


<a id="org90e43ce"></a>

### man


<a id="org719adf6"></a>

### grep


<a id="org77432fa"></a>

### nano


<a id="org00a7b9f"></a>

### curl & wget


<a id="orga59a252"></a>

### tar & zip


<a id="org56c797f"></a>

## 常用程序 (高阶)


<a id="org0911108"></a>

### Shell


<a id="org7d28577"></a>

### coreutils (扩展)


<a id="orgb2e7710"></a>

### sed


<a id="org9fd2a9d"></a>

### awk


<a id="orgaa3c7bb"></a>

## 扩展 (选修)


<a id="org236b778"></a>

### tmux


<a id="org1d8eebb"></a>

### 文件管理器


<a id="org57805cc"></a>

### Vim / Emacs


<a id="org96cc070"></a>

### systemd


<a id="org332c26f"></a>

## 扩展阅读


<a id="org5a660ef"></a>

### 常用程序的现代化替代品

<https://www.ruanyifeng.com/blog/2022/01/cli-alternative-tools.html>


<a id="org4b26593"></a>

# Bash 编程基础


<a id="orga2e1926"></a>

## 编写, 运行, 输入与输出


<a id="org9426ba1"></a>

## 基础控制流


<a id="org8e047b7"></a>

## 基础数据结构


<a id="org2f96588"></a>

## 脚本演示


<a id="org3061899"></a>

## 扩展阅读
