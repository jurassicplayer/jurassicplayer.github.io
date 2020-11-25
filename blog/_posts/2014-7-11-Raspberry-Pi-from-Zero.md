---
layout: post
title: Raspberry Pi from Zero
tags: [Archlinux, Linux, Management, Raspberry_Pi, Setup]
---

I thought I would make a post, or rather a log of setting up my raspberry pi in the event that I have to start over from scratch. Of course, this will mainly be covering the finer details of setting up the raspberry pi, since it's pretty easy to install via NOOBS. While NOOBS is probably easiest, the option is available to install just a certain distribution such as Pidora, Raspbian, etc. if you really aren't planning on using anything extra. In any case, I'm using the NOOBS lite version since I'm planning on having a cable anyways to update the install. With that said, the rest of this post is under the assumption that you have just installed Arch on your raspberry pi and are on the very first boot of your new installation.

The arch install itself took my raspberry pi around 4-5 minutes and the default login is root/root and probably the first thing to do is to update the system with a simple "pacman -Syu". After updating everything, rebooting is a good idea to make sure everything is reloaded and nothing strange happens.

Then we set the root password with "passwd" and change the hostname as well to something we could use to differentiate or just for laughs with "nano /etc/hostname". Just remember to add that hostname to the /etc/hosts file ("127.0.0.1 localhost.localdomain localhost <hostname>).
Continuing on, we remove /etc/localtime via "rm /etc/localtime" and link to a new one with "ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime". Of course, change the America and Los_Angeles bit to match your local time (to list out all of the options, just "ls /usr/share/zoneinfo/...").

We could literally call it quits here and just use the root account for everything, but as anyone who knows what they are doing, you NEVER just casually use root for anything. So, we need to create a new user and give him almighty powers that he can use whenever he wants. This can be done with "useradd -m -G wheel -s /bin/bash <username>" and to break the flags down, -m is to make a /home/username directory to go with the new user, -G is the group or groups the user is in, and -s is what shell they will use. Now we need to give the user a password, thus "passwd <username>" to change that to something memorable.

To give root access to users, we are going to need to grab "sudo" from the repo, so "pacman -S sudo" will install that in a snap. Then we open the permission settings for sudo, we are going to use "EDITOR=nano visudo", though if you like using vi as a text editor, that is fine too. Scroll down to "# %wheel ALL=(ALL) NOPASSWD: ALL" and uncomment it by removing the "# " in front of it. Feel free to read the descriptions for the options in the event that you don't want to have the users in wheel to just have unrestricted root access (ie. they need to use their password, etc.)

For the sake of clarity, you don't need to use the wheel group, you can also use the sudo group or a totally different group if you want. Just remember to have the user in that group and that the sudo permissions also reflect that.

Next is to set up the time to sync with the internet time, thus we need to enable ntp with "timedatectl set-ntp true" and things should supposedly automagically work.

Now we can log out of root and log into our new user account. At this point, we have a fully prepared Archlinux install that is ready to become the testbench of the flood of programs we are going to install to create our media server/aggregator. Since this is a lengthy post as it is, and I would rather re-read this stuff on separate posts, I'm going to do just that.
