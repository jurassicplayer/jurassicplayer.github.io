---
layout: post
title: Raspberry Pi from Three
tags: [Application, Archiving, Archlinux, File System, Linux, Management, Raspberry Pi, Setup, Torrent]
---

So with a great file system on your hard drive, we need to do a quick addition to our /etc/fstab to mount it at boot, but first is a recap of the other stuff we still need to do.

- RSS aggregator of some sort to sift through the torrents we are looking for and grab the ones we want
- A torrent client to actually get them without killing our pi
- A protocol to serve our media to another device on the network
- Maybe something to hide ourselves so we aren't blatantly telling the world that anime is all we ever want

Now to mount our drive at boot. Just pop open /etc/fstab with something like "sudo nano /etc/fstab" or any other preferred text editor. You will need to know where your block device is (/dev/sda1, /dev/sda2, /dev/sdb1, etc.) and just slap an entry along the lines of 
    /dev/sda1     <some mount point>   <file system type (ie. ext4, ntfs, )>         defaults       0     0
If your device mounts and the permissions are wrong, you might not be able to write anything unless you use root. When this happens, add "defaults,uid=1000,gid=1000" or whatever options you are mounting your drive with. And with that, we can finally continue towards tackling the next major issue on our list of things to deal with, the RSS aggregator.

There is a small number of programs at our disposal that we can use to deal with this problem, with the most notable ones being sickbeard (not really) and flexget. After that there are some others like rssdler, couchpotato, etc. that I didn't look into. Aside from these, there are also some RSS aggregators that are included (usually as a plugin) in torrent clients like deluge or rutorrent (frontend to rtorrent).

Sickbeard's support for anime is shoddy at best, with the main way being a branch of sickbeard that isn't really maintained, making it kind of pointless...and while flexget is better, I just never liked flexget and so I didn't try it. Rssdler might have been a good choice, since it looked pretty lightweight to match rtorrent, but after 5 years of next to zero development activity, it's pretty much a dead project.

Thus, I really looked towards deluge and rtorrent as being the grand solution. Setting up and preparing deluge is amazingly easy and primarily relies on a plugin called YaRSS2. Simply install deluge, start the daemon, connect with a deluge client, and install the plugin. After that, you are just about ready to start reading rss feeds and auto-downloading the torrents. Since the wiki is probably loads better than any explanation I could pull off (it has examples), the link is here. Rtorrent on the other hand...has you going through a couple of hoops, but in my opinion, those hoops are just barely justified.

When using deluge, you would need to use a client and while there is a plugin for a webui, it unfortunately doesn't have a webui for YaRSS2 which is our primary way of automating our anime downloads, meaning we would either need to utilize a script which would add a new entry into the YaRSS2 config after we type in the anime title somewhere or use the gtk client. At the point where you are going to just use the gtk client to connect to the deluge server, you might as well just set up the server on your computer and use deluge in classic mode. Rutorrent on the other hand has an RSS manager plugin and of course, since the whole frontend is a webui, the plugin also has a web interface. This means that when you check your Raspberry pi's torrents, you can manage every torrent-related thing from a single webpage without needing a client (managing it on-the-go).

Since setting up rtorrent+rutorrent is a bit of a journey, that will have to be in the next post.
