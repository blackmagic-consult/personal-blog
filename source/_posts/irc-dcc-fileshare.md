---
title: Utilizing IRC & DCC As a Filesharing Medium
date: 2018-02-09 09:45:03
tags:
---

The concept of Filesharing (and digital piracy) has been around since before even the basic days of the internet - where floppy disks, and, even earlier, rolls of punch-paper, were shared at swap meets.

The Internet, as it is always keen to do, revolutionized filesharing in a big way. Bulletin Board Systems was where shareware began to congregate, where the very idea of the "scene" really began. Eventually, the Scene spread to Usenet, then FTP & FXP. In the late 1990's, IRC became the system of choice, as development in the DCC protocol made it significantly faster compared to other methods.

While Bittorrent stands out now as the be all end-all for filesharing, killing Usenet, and neutering the usefulness of services such as Napster or Nicotine, IRC and the DCC protocol still are actively used and developed; sharing files via IRC is still a viable way to find and download files - especially rare ones - that stays on par with modern expectations in speed. <!--more-->

## Foreword

This guide explains how to enable DCC in Weechat. This process, while simple, is, in my findings, not greatly documented.

If you are using another client, like Hexchat, I'm afraid you're left to your own devices in regards of DCC setup. 

## 1: Enable DCC

Weechat comes with the plugin needed installed (at least on Arch Linux), but disabled. To enable DCC receiving, type

```Weechat
/plugin load xfer
```
## 2: Warez Channels

Files are distributed either peer to peer, or via a XDCC bot. These bots congregate in warez channels.

While some of these channels are regularly [scraped](http://www.xdcc.eu), most of the ones of any value are very hard to find and will require an invite from someone who is already in the know.

Ask around in your favoured IRC channels. Someone will point you in the right direction.

## 3: XDCC Bots

The hardest part is already over; XDCC bots are very simple to interface with. Type `!list` in channel, and a bot will PM you a list of what files it has to download. Follow its instructions to begin a transfer.

Some bots will not allow you to ask it for a file list. In that case, wait in channel and eventually it'll list what packs it has available.

Easy as bread.

A final pointer, one I hope I should not have to state, but will to cover my ass: Be respectful. Don't eat up bandwidth grabbing files you don't need. Don't clutter chat with commands. Don't spam the bot with `!list` requests.

Be a decent human. Pirate with courtesy. When you can, support the creators you love.