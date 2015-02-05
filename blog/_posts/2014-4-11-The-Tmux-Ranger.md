---
layout: post
title: The Tmux Ranger
tags: [Application, Blurb, CLI, compositing, compositor, Eyecandy, Linux, Management, ranger, terminal, tmux, transparency, urxvt]
---

So the tmux + ranger duo. It's something I have coveted for quite some time, though to be honest I almost never use it anyways (but it's just that awesome). Tmux is a terminal multiplexer and that means that it can split up your terminal into multiple terminals in a single window. Ranger on the other hand is a badass cli file manager that you can add pretty much any shortcut in and do tons of other nifty things with. The problem that was present before was that combining the two generally resulted in the w3m image displaying capabilities to die, meaning those wonderful true color images couldn't be seen in ranger while using tmux. To someone like myself, that SUCKS...ASCII is great and all, but I WANT my true color images. Apparently the issue was that the standard way of getting the terminal size normally wasn't possible in tmux due to how it can be resized and stuff (and thus instead of reporting decent information, it reported pretty much zero).

Eventually a guy was like "w3m works in tmux...let's check what they do" and lo and behold, there was a option along the lines of "-test" that was used by w3m to check the size. Next thing you know, there is a small patch, some little fixes, another fix to deal with the derpiness that urxvt had, and bam a workable patch to deal with tmux and the image viewing problem that ranger had. At the current time, the main Arch repos have an older version of ranger, but that is why there is the all-mighty AUR which had the "pull shit down from git and build it for maximum bleed" versions (ranger-git is apparently tagged outdated for something...as to what I have no idea since it still builds and all...worst case, just use the python2 version I guess...). Thus the AUR has a working version of ranger that can display true color images in tmux.

The next issue deals with compositing, because apparently there is also an issue where ranger can't show images when the terminal has compositing on. This means it's either semi-transparent terminal without images or an opaque terminal with images. As far as I can tell, there is no "real" solution for the issue so getting that true transparency terminal AND images in ranger just isn't going to happen.

A "solution" provided by a fellow Arch user consisted of (added to .bashrc):
    
    alias ranger='nohup urxvt -depth 16 -e ranger && urxvt & exit'

It pretty much just says to change ranger to open up a urxvt terminal with true transparency off entirely so that when ranger is called, you get a solid terminal and those wonderful true color images while closing the previous terminal and then re-opening urxvt after ranger closes out.

In other words, it makes things ugly just to get a ranger terminal out. Not only that, but it also prevents ranger from opening in a tty as well. Of course, I have the best solution because otherwise I wouldn't need to make this post. Granted it is not much better than the command above, but it will give you the next best thing.
    
    alias ranger='if [[ $DISPLAY == :0 ]]; then urxvt -depth 24 -transparent -shading 25 -e ranger &disown; elif [[ -n "SSH_CLIENT" ]]; then ranger --cmd="set preview_images false"; else ranger; fi' 
    
It just adds a quick check if $DISPLAY was set and to spawn urxvt+ranger if it is, or if the tty session is through a ssh connection and just use ascii images, otherwise just running ranger if it isn't. Of course, the spawned terminal also has fake transparency because if I can't have true transparency, I'll settle for some fake.

Pretty much, it seems that in an ssh connection without X, the $DISPLAY isn't set, a ssh with X does get set (though since I just ssh'd into the same computer...it might be set to something than what I saw, but it was "localhost:10.0") but it doesn't really matter since the second if statement catches all ssh connections. It's somewhat important to differentiate between ssh and normal because you can't pass the framebuffer over ssh without using ssh -X (and since I never do ssh with X, I have no idea how it works). The final bit catches the rest of the ranger calls, which would maybe be tty1-9 etc. on the local computer and spawn ranger normally without the extra urxvt.
