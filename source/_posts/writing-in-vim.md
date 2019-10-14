---
title: Writing in Vim
date: 2018-01-28 23:42:02
tags:
---

Recently, I uploaded a [Vim plugin](/2018/01/28/write-as-vim/) that lets you post to write.as directly from vim. The response to it was great, but I got some questions from readers regarding my writing process in such a bare-bones application such as Vim.

Here is how I do it. <!--more-->

## Precursor

This guide will not go into heavy detail regarding how to use Vim in its most basic form. 

If you need a primer on how to use Vim,  type `:h tutor` into Vim, or check out [openvim](https://openvim.com).

This guide assumes you are starting out with a clean *.vimrc*, as well, but can easily be applied if you already are comfortable using vim.

## 1: The Plugin Manager

I use [vim-plug](https://github.com/junegunn/vim-plug), as in my testing it is significantly faster than both [Vundle](https://github.com/VundleVim/Vundle.vim) and [Pathogen](https://github.com/tpope/vim-pathogen), but the differences are minimal. Use whatever manager you like.

Installing any of them is as easy as moving a *.vim* file into your *~/.vim/autoload/* directory. In the case of vim-plug, it is as easy as running curl:

```curl
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

## 2: The Top of *~/.vimrc*

This is a must. Ensure the very top of your *~/.vimrc* is this:

```VimScript
set nocompatible	
" Enforces vim defualts instead of vi
set encoding=utf-8	
" Sets encoding to utf-8
scriptencoding utf-8	
" Sometimes the above fails. This corrects that.
```

## 3: Installing The Plugins

You need to call plug-vim, define where it should place its plugins, then list the plugins you wish to install. In short, put this near the top of your *~/.vimrc*:

```VimScript
call plug#begin('~/.vim/plugged') 
" Call plug and define the plugin folder
	Plug 'reedes/vim-pencil'
	" The mother plugin which makes this all possible
	
	Plug 'junegunn/goyo.vim'
	" Turn vim into a distraction free editor

	Plug 'junegunn/limelight.vim'
	" Make editor even cleaner
	
	Plug 'reedes/vim-wordy'
	" Make your writing better
	
	Plug 'reedes/vim-litecorrect'
	"Annoyance free autocorrect

	Plug 'rhysd/vim-grammarous'	
	"A clean grammar checker.

	Plug 'internationa1/write-as.vim'	
	" Upload straight to write.as

	" Ensure all Plugins are added above the following line
call plug#end()
```

## 4: General Settings

Filetype detection must be enabled for some of the plugins to auto-enable, and for markdown syntax to be detected.

```VimScript
" File detection
filetype on
filetype plugin indent on
syntax on
```

Line numbers are convenient for editing, autocompletion is awesome, unloading buffers are a pain, mouse use is nice, and word wrap is soft handled via vim-pencil. So we'll enable and disable functions accordingly.

```VimScript
set number	"Enable line numbers
set completopt=longest, menuone	 " Autocomplete options
set complete=.,w,b,u,t,i,d	
"Autocomplete options, for more info :h complete
set hidden	"Don't unload buffers
set mouse=a	"Enable mouse
set nowrap	"Disable nowrap
set showbreak="+++ "	" string to show with wrapped lines
set ttyfast	" Render characters quicker
set title	" Use filename in window title
set wildmenu	" Enhance command line completion
set wildchar=<TAB>	" Set tab for command completion
set noerrorbells	" Noises are annoying
```

Vim 8 comes with baked-in spellcheck. You are running Vim 8, right?

```VimScript
set spellang=en_gb	
"Set spellcheck language. You might want en_us, or en_can

set spellfile=~/.vim/spell/en.utf8.add	
"The dictionary file

hi clear SpellBad
hi SpellBad cterm=underline ctermfg=red	
" Underline spelling errors in red

hi clear SpellCap
hi SpellCap cterm=underline ctermfg=blue	
" Underline spelling warnings in blue

hi clear SpellLocal
hi SpellCap cterm=underline ctermfg=green	
" Underline localization errors in green.
```
## 5: Autocommands

Markdown has some particulars when it comes to syntax, so we'll specify what to do when a markdown file is open.

```VimScript
augroup markdown
autocmd!
autocmd FileType markdown,mkd,md
 " Do this when you open a markdown file
	\ setlocal noexpandtab tabstop=4 shiftwidth=4 softtabstop=4
	\ list lcs=tab:+.
	\ set spell spelllang=en_gb	" Enable Spellcheck
	\ call litecorrect#init() 	" Enable autocorrect
	\ call pencil#init	"Enable vim-pencil
	\ setlocal textwidth=80	" Match write.as width-ish
augroup END

autocmd! User GoyoEnter Limelight
autocmd! User GoyoLeave Limelight!
" When Goyo is in use, enable limelight.

" Writeas User Info
let g:writeas_u = 'YOUR_USERNAME'
let g:writeas_p = 'YOUR_PASSWORD'
let g:writeas_b = 'YOUR BLOG'
```

## 6: You're set.

When you open a markdown file, spellcheck will turn on, your tab settings will initialize, autocorrect will kick in, vim-pencil will start, and the width will be set to (roughly) what write.as is set to.

When you turn on Goyo - which you do by typing `:Goyo` - limelight will itself turn on, and when you leave, it will turn off. In short, Vim now looks like this:

![Vim Image](https://kek.gg/i/6Z33kX.png)

Checking for weak wording in your writing is now as easy as typing `:WeakWordy`.

When you're all finished with writing, uploading to your blog is as easy as typing `:BlogPost TITLE`. You're now a hardcore writing machine.

## Afterword

It would have been a lot faster for everyone following this guide if I had just uploaded my *~/.vimrc*, but I didn't, for a handful of reasons.

Mainly, while I know a lot of people who use Vim, I know significantly fewer who actually understand vim script - this is fair, because vim script is pretty hard to work with. Copying a dotfile off of github is a lot faster, but knowing how the software you rely on works, and what it's quirks are, is important to being a better hacker.

Following this tutorial means you built your own *.vimrc*. You now see how simple and versatile VimScript actually is. Now, extending vim to be a Python or Lua IDE is easy as installing your preferred plugins and setting up autocmds.


Maybe next you should setup [Airline](https://github.com/vim-airline/vim-airline) or setup Vim to manage your accounting via [Ledger](https://ledger-cli.org). Maybe write your own Vim plugin that will let you [preview how your blog post will look once uploaded.](http://www.terminally-incoherent.com/blog/2013/05/06/vriting-vim-plugins-in-python/) The limit is your imagination, since you now have the tools.