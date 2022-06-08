# vim要素
---
1. 不要使用插件管理器
2. 不要使用安装版vim
3. 不要使用其他编辑器

同样的， vim.o.showtabline = 2 也是固定设置，表示永远显示 tabline，因为后边也有对应的 tabline 插件，vim.o.hidden = true 在使用多个 buffer的时候是必需的，表示允许隐藏被修改过的 buffer。 否则会报 E37: No write since last change 错误，强制你保存当前buffer后才允许切换到其他的 buffer。

VIM笔记-编码（乱码问题最终解决办法）
晚辈曾阿牛
0.118
2016.07.31 14:13:52
字数 1,479阅读 5,144
献上我经过测试以后的最终解决办法

首先第一步找到VIM的配置文件，windows中的vim的配置文件是：

    C:\Program Files\Vim_vimrc

接着修改这个文本文档，文档前面的所有内容均不需要动，只要把下面这些代码加入到最后即可。

    set encoding=utf-8
    set termencoding=utf-8
    set fileencoding=utf-8
    set fileencodings=ucs-bom,utf-8,cp936,gb18030
    set imcmdline
    source $VIMRUNTIME/delmenu.vim
    source $VIMRUNTIME/menu.vim

尤其要注意，最后的两行代码，千万不能放到配置文件的上面。

解决consle输出乱码

    language messages zh_CN.utf-8

vim的编码问题

    首先要明确：

    Windows中默认的文件格式是GBK(gb2312)，而Linux一般都是UTF-8。

    在vim中查看文件的编码方式：set fileencoding ,即可显示文件编码格式，如果什么都没有显示，请看第三点。
    如果你只是想查看其它编码格式的文件或者想解决用Vim查看文件乱码的问题，那么你可以在~/.vimrc (如果是windows系统，那么应该修改_vimrc)文件中添加以下内容：
    set encoding=utf-8 fileencodings=ucs-bom,utf-8,cp936

    这样，就可以让vim自动识别文件编码（可以自动识别UTF-8或者GBK编 码的文件），其实就是依照fileencodings提供的编码列表尝试，如果没有找到合适的编码，就用latin-1(ASCII)编码打开。

    以指定的编码打开某文件，如打开windows中以ANSI保存的文件:
    vim file.txt -c "e ++enc=GB18030"
    文件编码转换,在Vim中直接进行转换文件编码,比如将一个文件转换成utf-8格式:
    :set fileencoding=utf-8
    6.如果已经打开了解码错的文件，想重新设置编码格式：
    :edit ++enc=utf8
    查看文件格式
    :set fileformat?
    设置文件格式为 unix
    :set fileformat=unix

如果有时间，请看下面的具体解释：

Vim 有四个跟字符编码方式有关的选项，encoding、fileencoding、fileencodings、termencoding (这些选项可能的取值请参考 Vim 在线帮助 :help encoding-names)，它们的意义如下:

    encoding: Vim 内部使用的字符编码方式，包括 Vim 的 buffer (缓冲区)、菜单文本、消息文本等。如你的vim的encoding为utf-8,所编辑的文件采用cp936编码,vim会自动将读入的文件转成utf-8(vim的能读懂的方式），而当你写入文件时,又会自动转回成cp936（文件的保存编码)。

    fileencoding: Vim 中当前编辑的文件的字符编码方式，Vim 保存文件时也会将文件保存为这种字符编码方式 (不管是否新文件都如此)。

    fileencodings: Vim自动探测fileencoding的顺序列表， 启动时会按照它所列出的字符编码方式 从前到后，逐一探测 即将打开的文件的字符编码方式。因此最好将Unicode 编码方式放到这个列表的最前面，如果都找不到，那么就会以 latin1 (ASCII)的方式打开。

    termencoding: Vim 所工作的终端 (或者 Windows 的 Console 窗口) 的字符编码方式。如果vim所在的term与vim编码相同，则无需设置。如其不然，你可以用vim的termencoding选项将自动转换成term的编码.这个选项在 Windows 下对我们常用的 GUI 模式的 gVim 无效，而对 Console 模式的Vim 而言就是 Windows 控制台的代码页，通常我们不需要改变它。

解释完了这一堆容易让新手犯糊涂的参数，我们来看看 Vim 的多字符编码方式支持是如何工作的：

    Vim 启动，根据 .vimrc 中设置的 encoding 的值来设置 buffer、菜单文本、消息文的字符编码方式。

    读取需要编辑的文件，根据 fileencodings 中列出的字符编码方式逐一探测该文件编码方式。并设置 fileencoding 为探测到的，看起来是正确的 (注1) 字符编码方式。

    对比 fileencoding 和 encoding 的值，若不同则调用 iconv 将文件内容转换为encoding 所描述的字符编码方式，并且把转换后的内容放到为此文件开辟的 buffer 里，此时我们就可以开始编辑这个文件了。注意，完成这一步动作需要调用外部的 iconv.dll(注2)，你需要保证这个文件存在于 $VIMRUNTIME 或者其他列在 PATH 环境变量中的目录里。

    编辑完成后保存文件时，再次对比 fileencoding 和 encoding 的值。若不同，再次调用 iconv 将即将保存的 buffer 中的文本转换为 fileencoding 所描述的字符编码方式，并保存到指定的文件中。同样，这需要调用 iconv.dll由于 Unicode 能够包含几乎所有的语言的字符，而且 Unicode 的 UTF-8 编码方式又是非常具有性价比的编码方式 (空间消耗比 UCS-2 小)，因此建议 encoding 的值设置为utf-8。这么做的另一个理由是 encoding 设置为 utf-8 时，Vim 自动探测文件的编码方式会更准确 (或许这个理由才是主要的 ;)。我们在中文 Windows 里编辑的文件，为了兼顾与其他软件的兼容性，文件编码还是设置为 GB2312/GBK 比较合适，因此 fileencoding 建议设置为 chinese (chinese 是个别名，在 Unix 里表示 gb2312，在 Windows 里表示cp936，也就是 GBK 的代码页)。

"小礼物走一走，来简书关注我"
还没有人赞赏，支持一下

