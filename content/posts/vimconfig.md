---
title: "Vim配置与推荐插件分享"
date: 2025-08-24
author: "Jiakuo"
categories: ["技术"]
tags: ["Vim", "配置分享"]
description: "分享作者的vimrc"
showToc: true
tocOpen: true
draft: false
---
### 前言

笔者此时正使用vim撰写此网站的第一篇文章，因此决定将它献给vim。

vim素有编辑器之神的美誉，初次遇见它我便对其编辑模式爱不释手。

在花了很长的一段时间熟悉了vim的使用之后，我将电脑里的其他编辑工具都安装了vim的插件（已经是vim的形状了），比如在vscode当作安装VSCode Neovim，在Visual Studio中安装了VsVim等，之后便开始旷日持久地折腾起vim的配置，使其符合我的编辑习惯，到现在vim已经是我日常写代码、Debug的最佳伙伴。关于使用vim的好处不在此赘述，下面介绍笔者的vimrc配置（这里仅提供Vimscript。如使用Neovim，可以直接将下面的配置复制粘贴给ChatGPT一类的工具，将其转换为等价的Lua语言版本）。

### 键位设置

关于键位remap的问题，首先使用空格作为leader键，这点没什么可说的（听说有人使用逗号`,`作为leader，这简直是vim神教的异教徒）。之后便是一些快速调用插件、调整窗口尺寸、窗口间光标移动等的键位设置。注意到笔者的设置几乎都使用了`silent`这一关键字，这样可以避免在实际的使用过程中出现命令的回显，干扰我们的视觉。

我使用`Ctrl + {↑, ↓, ←, →}`来实现分屏时各个窗口大小的调整，使用`Ctrl + {h, j, k, l}`来实现光标的跳转，使用`space + {↑, ↓, ←, →}`来实现一些Tab操作。一些常用的编辑功能我喜欢用`space + 字母`来作为快捷键，比如`space + x`（即下面的配置中使用正则表达式替换的功能）用来删除所有行尾的空格（这在一些语言的lint工具当中常作为Error被提示）。

另外我喜欢用`H`和`L`来代替正则符号的`^`和`$`（即跳至行首/行尾，无他，因为那默认的那两个键实在是不好按）。

```vimscript
let mapleader = " "

" NERDTree navigation
nnoremap <silent> <leader>n        : NERDTreeToggle<CR>
nnoremap <silent> <leader>f        : NERDTreeFind<CR>

" Window resize
nnoremap <silent> <C-Up>           : resize +5<CR>
nnoremap <silent> <C-Down>         : resize -5<CR>
nnoremap <silent> <C-Left>         : vertical resize +5<CR>
nnoremap <silent> <C-Right>        : vertical resize -5<CR>

" Quick actions
nnoremap <silent> <leader><leader> : nohlsearch<CR>
nnoremap <silent> <leader>p        : set paste!<CR>
nnoremap <leader>x                 : %s/\s\+$//e<CR>
xnoremap <leader>a                 : Tabularize /=/<CR>

" Cursor move between windows
nnoremap <silent> <C-k>            : wincmd k<CR>
nnoremap <silent> <C-j>            : wincmd j<CR>
nnoremap <silent> <C-h>            : wincmd h<CR>
nnoremap <silent> <C-l>            : wincmd l<CR>

" Tab pages
nnoremap <silent> <leader><Up>     : tabnew<CR>
nnoremap <silent> <leader><Down>   : tabclose<CR>
nnoremap <silent> <leader><Left>   : tabprevious<CR>
nnoremap <silent> <leader><Right>  : tabnext<CR>

" Move in line
nnoremap H ^
xnoremap H ^
onoremap H ^
nnoremap L $
xnoremap L $
onoremap L $
```

### 常规设置

常规设置里没有什么可说的，这些大多都是吸收前人的经验，包含一些可以让vim变得更好用的非常基础的config。
另外就是vim的主题设置也在这里进行，笔者使用的是非常经典的`gruvbox`（谁不想主题看起来漂漂亮亮的呢）。

```vimscript
language en_US
set nocompatible
filetype on
filetype plugin on
filetype indent on
set encoding     =utf-8
set history      =1000
set scrolloff    =10
" set laststatus =2
" set statusline =%F\ %m%r%h%w% = %y\ [%p%%]\ [%l,%c]
" set mouse      =a
" set nobackup

" Indentation
set autoindent
set shiftwidth  =4
set softtabstop =4
set tabstop     =4
set expandtab

"  Search Behavior
set hlsearch
set incsearch
set ignorecase
set smartcase

" Display & Appearance
colorscheme gruvbox
set background=dark
set showcmd
set showmatch
set cursorline
set number
set nowrap
set ruler
set splitright
set splitbelow
syntax on
set t_Co=256
" set termguicolors
" set showmode
" set relativenumber
" set cursorcolumn
" let g:netrw_bufsettings = 'number'
" let g:airline_theme='gruvbox'

" Menu Completion & Ignore
set wildmenu
set wildmode   =list:longest
set wildignore =*.docx,*.jpg,*.png,*.gif,*.pdf,*.pyc,*.exe,*.flv,*.img,*.xlsx
```

### 插件设置

最后来到的插件部分的介绍，笔者使用vim-plug来进行插件的管理（感谢开发者，使用起来非常方便）。
至于插件内容，我信奉`Small is Big`准则，从实用角度出发仅安装了数个插件。包含主题显示、代码高亮、文本按字符对齐等功能的插件。

值得一提的是，vim-plug的[GitHub页面](https://github.com/junegunn/vim-plug)推荐使用curl等网络工具命令来进行下载，但实际上当我们的工作在一些机密度很高的服务器上进行时，出于数据安全的考量通常无法直接访问外部网络，这时候可以通过先本地下载`plug.vim`，再使用一些ssh工具将其传送到服务器上来进行vim插件的管理。当然，插件本身也需要进行同样的操作，去到他们的GitHub页面下载源代码，传送至远程服务器即可。

```vimscript
" --------------------
" Plugins(via vim-plug)
" --------------------
"  Download plug.vim and put it in "~/.vim/autoload/".
"  Download the ZIP file of Plugins and extract it to "~/.vim/plugged/".
call plug#begin()

Plug 'preservim/nerdtree', { 'on': 'NERDTreeToggle' }
Plug 'vim-airline/vim-airline'
Plug 'sheerun/vim-polyglot'
Plug 'morhetz/gruvbox'
Plug 'godlygeek/tabular'

call plug#end()
```
