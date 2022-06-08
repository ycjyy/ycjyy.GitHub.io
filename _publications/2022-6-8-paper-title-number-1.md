---
title: "Paper Title Number 1"
collection: publications
permalink: /publication/2009-10-01-paper-title-number-1
excerpt: 'This paper is about the number 1. The number 2 is left for future work.'
date: 2009-10-01
venue: 'Journal 1'
paperurl: 'http://academicpages.github.io/files/paper1.pdf'
citation: 'Your Name, You. (2009). &quot;Paper Title Number 1.&quot; <i>Journal 1</i>. 1(1).'
---
This paper is about the number 1. The number 2 is left for future work.

[Download paper here](http://academicpages.github.io/files/paper1.pdf)

Recommended citation: Your Name, You. (2009). "Paper Title Number 1." <i>Journal 1</i>. 1(1).

官方编译：GitHub vim-win32-install 如果觉得官方版开启的特性无法满足需求，那我们就需要自己编译 Vim 源码。
工具

    Visual Studio
    Windows SDK
    Vim 源代码（GitHub Release）

非等宽字体支持

进入源码目录下的 src/ 目录，找到 Make_mvc.mak 文件并打开。在 370 行的 CFLAGS= 后添加参数 -DFEAT_PROPORTIONAL_FONTS 使编译出的 Vim 支持非等宽字体。添加后效果如下：

1
2
3
4
5

	

CFLAGS = -DFEAT_PROPORTIONAL_FONTS -c /W3 /nologo $(CVARS) -I. -Iproto -DHAVE_PATHDEF -DWIN32 \
        $(CSCOPE_DEFS) $(NETBEANS_DEFS) $(CHANNEL_DEFS) \
        $(NBDEBUG_DEFS) $(XPM_DEFS) \
        $(DEFINES) -DWINVER=$(WINVER) -D_WIN32_WINNT=$(WINVER) \
        /Fo$(OUTDIR)/

当然也可以通过注释源码的方式实现支持非等宽，只是相对于修改 Makefile 修改源码较为麻烦。
DirectWrite 抗锯齿

GVim 在 Win 下的字体渲染效果较差，没有 Mac 和 Linux 的 GVim 看起来平滑细腻。虽然 Win 下 GVim 支持 DW 抗锯齿，但默认并没有开启。要开启抗锯齿功能不仅需要在编译时增加参数，还需要在 vimrc 中设置。
按照 Makefile，编译时提供参数：

DirectWrite=yes

编译完成后打开 _vimrc 并添加：

1
2
3

	

if has('win32') || has('win64')
    set renderoptions=type:directx,renmode:5
end

重启 Vim 后就可看到字体渲染效果显著提高。
注：此方法会增加 vim 对显卡的需求，在当前屏幕的代码量非常大的时候可能会降低 vim 的翻页速度。
特性

在 Vim 中输入命令 :version 就可以看到当前编译版包含的所有特性：加号为支持，减号为不支持。我们可以在编译打开对应的开关以支持某种功能或关闭开关减少编译得到的二进制文件的大小。
FEATURES

除了单个功能的开关之外，Vim 还提供了 FEATURES 选项以快速开启或关闭某一组特性，可选：TINY（体积最小）、SMALL、NORMAL、BIG、HUGE（体积最大）。如果对空间没有特殊要求推荐选择 HUGE。
脚本支持

Vim 提供的 VimScript 功能较为简单，运行速度也不快，所以许多插件开发者选择使用其它语言的脚本开发 Vim 插件。在使用这些插件的时候我们的 Vim 需要支持对应的脚本语言。在编译时传入解释器所在目录即可使 Vim 获得对对应语言的支持。DYNAMIC_LANGUAGE 则决定脚本语言模块于何时加载，如果设置为 yes 则只会在第一次运行脚本时加载语言模块，如果设置为 no 则在 Vim 启动时加载语言模块。

附上我使用的 Vim x64 编译参数（支持 python，python3，lua。需要提前安装好对应环境并修改参数中的目录）：

1

	

nmake -f Make_mvc.mak CPU=AMD64 SDK_INCLUDE_DIR="C:\Program Files (x86)\Microsoft SDKs\Windows\v7.1A\Include" FEATURES=HUGE GUI=yes DIRECTX=yes OLE=yes MBYTE=yes IME=yes GIME=yes LUA=F:\lua LUA_VER=53 PYTHON=E:\DeveloperTools\Python27 PYTHON_VER=27 PYTHON3=E:\DeveloperTools\Python35 PYTHON3_VER=35 DYNAMIC_PYTHON=yes DYNAMIC_PYTHON3=yes DEBUG=no DYNAMIC_LUA=yes XPM=no

注

    需要 +Python 特性时，不要使用 Python 2.7.11。此版本会导致 Vim 无法加载 site-packages 导致所有需要 Python 的插件失效。
    使用 Visual Studio 编译 GVim 并添加 +lua 支持时，如果 lua 是从 LuaForge Lua Binaries project 下载的动态库，需要在 lua 根目录下创建 include 文件夹并将 lua<version>.lib 文件移动到 include 中。否则连接时会提示找不到 lib 文件。

Vim
上一篇：
对 Git 使用 SSH 代理

Disqus seems to be taking longer than usual. Reload?

Github 名片

标签

    Git2
    C/C++2
    Android1
    Vim1
    Firefox1

RSS 订阅

Hello ,I'm IceNature.

Powered by hexo and Theme by Jacman © 2018 IceNature


