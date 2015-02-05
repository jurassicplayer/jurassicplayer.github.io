---
layout: post
title: Raspberry Pi from Three and a Fourth
tags: [Application, Archlinux, Linux, Management, Raspberry_Pi, Rtorrent, Rutorrent, Setup, Torrent]
---

So getting rutorrent onto a raspberry pi is not nearly as amazingly easy as deluge, but hopefully it will be better in the end. Since a lot of the typing work has already been done, I'll just link to the arch wiki which should get you most of the way there. Just make sure to copy the template .rtorrent.rc to your home directory with "cp /usr/share/doc/rtorrent/rtorrent.rc ~/.rtorrent.rc".

After following the wiki, it brings you to the point where things almost fully work, but the log of rutorrent (when you open it in a web browser) gives you some of the issues it encountered.

The first of these might be something along the lines of the rtorrent user needing to have read/write permission to the tmp directory. Now I'm no expert in dealing with permissions, but to make things simple and quick to fix, just make a new directory in the /tmp folder with "sudo mkdir /tmp/rt" and then change it's permissions with "sudo chmod 777 /tmp/rt". Finally, taking a quick visit to /srv/http/rutorrent/conf/config.php, at the very end there is a line that says "$tempDirectory = null;" which you change to "$tempDirectory = '/tmp/rt';". Instantly problem solved.

The other issues that probably got logged were about the plugins. Some of these plugins you might want to disable (like the rutracker_check one), and if you want the other plugins (mediainfo, screenshots, unpack plugin), just install the packages that they are asking for (mediainfo, ffmpeg, unzip, and unrar respectively). The mediainfo, unzip, and unrar packages are pretty small in comparison to ffmpeg, and have their uses in other places as well...ffmpeg on the other hand is about 120MBs, but I'm still going to probably install it anyways. Now is also a good time to take a look through all of the plugins available in rutorrent (all the non-3rd-party ones anyways) and disable unneeded ones. It's not a huge hit in performance leaving them on, but meh, I'll take anything to lighten the load.

Now these problems are only what rutorrent can show you. There are more problems hidden in the /var/log/lighttpd/error.log that you can also spend time fixing :D. Most of the fixing is going to be in the /etc/php/php.ini, so open that up somehow and we want to add some extra directories to that open_basedir line after "/usr/bin/" to get our disk space plugin to work. Just change the end to "...:/usr/bin/:/:/usr/local/bin/:/usr/local/sbin/" but don't close the config file just yet.

Next is to fix the issue that get's spammed into the lighttpd error log about the time not being set. If you are using nano, use ctrl+w to open the search function and search for "date.timezone". This will bring you to the setting in the config where you want to uncomment the ";date.timezone =" and add your timezone to the end. In my case, I change it to "date.timezone = America/Los_Angeles" and the value for you is just the same as when you set up your timezone on the raspberry pi. After that, you can save the config, kill any php-cgi that might be running, maybe reload the systemctl daemon, restart lighttpd and give your rutorrent install another check.

Finally, we can clean up the horrid mess we made of everything and make things a bit nicer with starting at boot.
