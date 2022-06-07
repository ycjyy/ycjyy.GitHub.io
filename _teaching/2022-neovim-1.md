
---
title: "neovim"
collection: teaching
type: "Undergraduate course"
permalink: /teaching/2022-neovim-1
venue: "University 1, Department"
date: 2022-06-07
location: "City, Country"
---




其实nvim没必要用第三方配置，自己折腾就行。因为nvim性能很好；不像emcas一样自己折腾的配置不用懒加载会比doom慢一截。nvim不需要任何懒加载，只要你尽量装lua插件，少装viml插件，启动性能根本不会差。而且用nvim不需要像emacs那样装那么多插件，基本上legacy的vimL插件比如vim-repeat，vim-surround啥的装个五六个，剩下的基本都装lua插件了。无非就是statusline，terminal emulator，treesitter，lsp有关的插件，大部分在nvim都有非常主流热门的插件，第三方配置也是帮你预装这些。这点就不像emacs，啥都能干，干啥都需要配很多插件，nvim专注于文本编辑。我开启了impatient.nvim以后，启动时间120ms。
