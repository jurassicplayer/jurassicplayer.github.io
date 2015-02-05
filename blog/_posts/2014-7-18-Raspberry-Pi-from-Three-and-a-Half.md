---
layout: post
title: Raspberry Pi from Three and a Half
tags: [Application, Archlinux, Bash, Linux, Management, Raspberry Pi, Rtorrent, Rutorrent, Setup, Torrent]
---

So with our rutorrent and rtorrent set up, we can now move on to tackle more first world problems and start this stuff on boot. The main goal is to get lighttpd and rtorrent up and running when the pi starts up, so that we can instantly go to rutorrent after it finishes booting, and then do a little bit of configuration to rtorrent and rutorrent to smooth out some of the edges.

First is to install tmux or screen, though which one in particular is a toss-up (and I prefer tmux). After that, we need a systemd service to start tmux/screen and rtorrent, as well as stop it. Of course the arch wiki has a nice entry that you can pretty much copy/paste, though I like keeping the rtorrent as the full name for things rather than rt. In any case, "sudo systemctl enable rtorrent@user.service" and similarly for lighttpd.service will start both at boot so you don't have to do that manually and you can check your rtorrent by ssh'ing in and opening that tmux/screen session.

We are going to be taking a look at the .rtorrent.rc file to add a couple of extras that we'll need and other things. As a warning, pretty much all of the configurations in the .rtorrent.rc are also available in the Rutorrent webui, but for whatever reasons (maybe some permissions problems somewhere) it doesn't seem to save those configurations across reboots. So things like the download directory (best to set to the highest folder in the hierarchy of your anime), session directory, port range, port random, encryption, etc. should be set using the .rtorrent.rc file (and you should set them). At the very end of this file, we are also adding two lines:

    execute = {sh,-c,/usr/bin/php /srv/http/rutorrent/php/initplugins.php &}
    system.method.set_key = event.download.finished,notify_me,"execute=~/.cache/rtorrent/rtorrent_notify.sh,$d.get_name="

The first line is to load the plugins for rutorrent when rtorrent starts up so our feeds start up. The second line on the other hand is COMPLETELY OPTIONAL and is pretty much just executing a little script called "rtorrent_notify.sh" after a torrent finishes downloading. Supposedly with a little magic and some bash, you can send a great little notification to your computer from your raspberry pi that tells you when a new episode is ready.

The rest of the configuration can all be done with rutorrent and some time should really be put to exploring all of the settings available, but there are some quick things that I will highlight:
- General>User Interface>Theme (the oblivion or dark themes look pretty nice)
- Ratio Groups>Any number
The first bullet is just a preference thing, but the second is half of the staple way of automatically removing torrents from rtorrent when it's finished downloading. It's fairly easy to understand, just name the ratio to any name, choose a minimum percentage that you want to seed back to your peers, maximum doesn't really matter, a minimum amount in MBs that you want to seed back, and for how long (or just ignore the time all together), and then set the action to remove. If you wanted to be a real asshole, you could set all of it to zero and then as soon as your file finishes downloading, it gets removed or you could be a decent person and seed until you have given as much as you have received. To complete this automatic removal, we need to make sure that every torrent gets set to this ratio, and we can do that with the Rules Manager (button right next to the help, second option).

Pretty much just make a new rule where "if the torrent label contains <blank>", set the ratio to your new ratio. And by blank, I literally mean just a blank entry. With that, almost everything is taken care of in terms of our torrent client and we can look at the way we are going to be automagically adding new torrents to our heavily under-powered torrent box.
