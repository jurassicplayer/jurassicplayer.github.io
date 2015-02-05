---
layout: post
title: Raspberry Pi from Three and Three Fourths
tags: [Application, Archlinux, BakaBT, Linux, Management, Nyaa, Raspberry Pi, Rtorrent, Rutorrent, TokyoToshokan, Torrent]
---

Finally we are at the end of setting up our magical torrenting client and all we need to do is have a way to add our anime to an rss watch list. Luckily enough, we have just the plugin that can deal with RSS feeds, but to keep things in order, first we need to decide on a feed to follow.

The normal sites that I assume are standard when it comes to searching for this kind of media: Nyaa.se, Tokyo Toshokan, BakaBT, and ShanaProject. Out of these, ShanaProject is probably the most anime-oriented and is kind of specifically a site just for tracking anime and has user specific feeds that you can use to follow and track different fansub groups for different anime. It's the default feed of choice for me, but there are perks to using the others as well. Something to note though, Shanaproject along with Tokyo Toshokan pretty much aggregate torrents from Nyaa (at least in terms of anime), so there isn't much point to using all three of them.

To give a quick run-down of the options aside from ShanaProject:
- BakaBT: A "community" that dislikes leechers and doesn't seem to have any RSS feeds, making automation a bit of a bitch.
- Nyaa: The defacto tracker for pretty much all of the fansub groups. If they aren't on Nyaa, they are just retarded somehow.
- Tokyo Toshokan: The best option for all types of media from Japan. It aggregates anime, manga, music, etc. and has a customizable RSS to boot. Also, it has some re-encoders (hi10ani.me) if they actually put up torrents.

Anyways, after picking a feed to follow, we just click on the nice little rss icon in rutorrent, add our first rss feed and give it a name. Clicking on the rss icon afterwards now will bring up the RSS manager where all things great and automated occur. It's pretty simple from there, just come up with a name for your new subscription (usually I just title it the name of the anime), a regexp pattern to look for (ex. "/Akame ga Kill.*\[FFF"), the feed to watch, and a subfolder to download it into if you want (it defaults to the downloads directory of rtorrent). After that, the ratio rule and group from the previous post will deal with removal.

At this point, if you added the anime for each running season, your setup would automatically download, seed, and remove your torrents for the entire season without you needing to do a thing. It is probably possible to add an extra script to automatically generate even the configurations for the rss, but I currently don't have a script for that (in due time, I will get that since there happens to be a rather nifty link available on [MAL](http://myanimelist.net/malappinfo.php?u=jurassicplayer&status=all&type=anime)).

To take rutorrent a little bit farther and make it actually decent as a torrent client for things like older anime series or episodes that are not on the latest feeds of Nyaa or Tokyo Toshokan, I made some extra search engines for the extsearch plugin (thanks to the wonderful and fairly simple examples of the other engines). Just save them into /srv/http/rutorrent/plugin/extsearch/engines/<searchengine>.php files and they should magically appear and be ready for use.

Nyaa: [link](http://pastebin.com/D5S6NCyi)
SukebeiNyaa: [link](http://pastebin.com/fLgXAAxu)
TokyoTosho: [link](http://pastebin.com/mS67ZqnX)

Anyways, with that we FINALLY have all of our torrent client/rss aggregator woes fleshed out and out of the way, but we still have one little extra that we can do. After setting up auto ssh authentication (this can be found anywhere on google) from the raspberry pi to your obviously linux-running computer, you can make a rtorrent_notify.sh script to send a notification or something to your computer for when a torrent completes. My script is really simple:
    ssh user@host 'DISPLAY=:0 notify-send "New episode ready!" "$1"'  

-Extra Tidbits-
There is another website that could be used for tracking anime releases called anitor.net but that website really isn't useful for automation either (so it's kind of like BakaBT without the community and other useless stuff). It's pretty much a site that shows the latest releases in a nice list and has some fancy scripting that is nice if you are doing things manually.

Some people care nothing for torrents and only want DDL, and that is available too at AnimeTosho. It's an automated service that takes the anime releases from Tokyo Tosho (no batch releases or really large files) and reupload them to some other hosts so people can DDL. It's also majorly convenient because if you just want the .ass from the video, that is also provided. 
