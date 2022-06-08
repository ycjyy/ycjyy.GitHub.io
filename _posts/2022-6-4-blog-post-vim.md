# vim要素
---
1. 不要使用插件管理器
2. 不要使用安装版vim
3. 不要使用其他编辑器

同样的， vim.o.showtabline = 2 也是固定设置，表示永远显示 tabline，因为后边也有对应的 tabline 插件，vim.o.hidden = true 在使用多个 buffer的时候是必需的，表示允许隐藏被修改过的 buffer。 否则会报 E37: No write since last change 错误，强制你保存当前buffer后才允许切换到其他的 buffer。
