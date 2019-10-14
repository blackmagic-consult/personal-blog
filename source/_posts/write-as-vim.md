---
title: Posting to Write.as via VIM
date: 2018-01-28 17:31:31
tags:
---

## Update - 2019-01-24

This plugin was excellent for when I was still posting to write.as, instead of running this personal blog. However, since I've switched to a self-hosted stack that interprets Markdown directly, I no longer have need for it.

It still works last I checked, but if Matt changes the API or the plugin otherwise breaks, I won't find out about it. So it's techincally unsupported. I'll still fix bug reports, though, so if you find this plugin particuarily useful, just shoot me an email.

Mind you, it's like, a hundred lines of python. Nothing about it is too tricky to fix.


## Introduction


I love write.as; It's quickly become my home for blogging after only a few hours of tinkering. However, I've always done my writing in VIM, and as clean as the web interface is, I still found myself doing all the grunt work in VIM and then pasting it onto the site.

Not very efficient. 

[@matt](https://write.as/matt) pointed me towards [the API](https://developers.write.as), and it was a cinch to write up some vimscript and python to post from VIM to write.as directly. <!--more-->

Hence, I wrote [**This Plugin**](https://github.com/Internationa1/write-as.vim)

## Installation

The easiest way to install this plugin is, as usual, via a package manager like [Pathogen](https://github.com/tpope/vim-pathogen) or [Vundle](https://github.com/VundleVim/Vundle.vim) (My personal favourite being Vundle)

Then, just add:

```vimScript
Plugin 'blackmagic-consult/write-as.vim'
```

and

```vimScript
" Writeas User Info
let g:writeas_u = 'YOUR_USERNAME'
let g:writeas_p = 'YOUR_PASSWORD'
let g:writeas_b = 'YOUR BLOG'
```

into the top and bottom of your *~/.vimrc*, respectively.

Then, while you're editing a markdown file in vim, it's as easy as executing

```vimScript
:BlogPost TITLE
```

To upload said file to your blog.

Anonymous posts are as easy as typing `:AnonPost Title` as well.


## Other Plugins

[vim-surround](https://github.com/tpope/vim-surround) is great for easily manipulating quotes and brackets.

[vim-pencil](https://github.com/reedes/vim-pencil) is a must have if you intend to 
do any sort of writing in vim whatsoever.

[goyo](https://github.com/junegunn/goyo.vim) Turns VIM into a slick distraction free writer akin to [WriteMonkey](http://writemonkey.com/).