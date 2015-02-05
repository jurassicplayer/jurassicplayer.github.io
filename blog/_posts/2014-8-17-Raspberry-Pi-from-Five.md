---
layout: post
title: Raspberry Pi from Five
tags: [Anime, anonymous, Application, Archiving, Archlinux, DNSCrypt, Gufw, HTTPS, Internet, IPTables, privacy, Raspberry Pi, SSL, Tomb, Torrent, TrueCrypt, VPN, Web]
---

So we are finally at the end of the great raspberry pi journey for media serving and the last thing we really need is a way to protect our precious collection. This can be encompassed into two main ideas: protecting what goes in and out of your computer via the network, and protecting the stuff on your filesystem. Most of this I can't go in-depth about, but the general idea is to have a firewall for filtering what stuff comes in, encrypt and proxy around stuff that goes out, and encrypt the files in your filesystem. I have next to no opinions for any of these since I do none of them :D. But I'll still list stuff off the top of my head that pertain to it.

Firewall:

- IPTables: Pretty much the baseline for firewalls on Linux, most of everything else is a frontend.
- Any of the IPTables front-ends (Gufw is apparently one of the popular ones)

Encrypting Traffic out:

- Https for everything (both firefox and chromium have plugins for HTTPS everywhere)
- SSL/TLS for everything (this and HTTPS are more about protecting your web browsing)
- DNSCrypt: Helps prevent spying and man-in-the-middle attacks because apparently your IP can still be leaked to the ISP even with https/vpn/ssl, and guess what, it just wraps your DNS conversation stuff in SSL as well :D.
- A VPN service...which costs money...Pretty much, they will encrypt all of your data that you send out and bring in. Apparently [PrivateInternetAccess](https://www.privateinternetaccess.com/) is one of the better ones that also allows torrent traffic.
- A seed box: It downloads for you and then you can get the files over ftp or something...aka connect your raspberry pi to a network that isn't yours and use ngrok to be able to connect to it...but you better be damn confident that it's not going to go down easy.

Encrypting File System:

- Once upon a time it was TrueCrypt...maybe there will be a derivative somewhere later on.
- [Tomb](https://www.dyne.org/software/tomb/): Not only is it witty, but it's quite good...as long as you don't lose your key and password...which you still need to hide somewhere.

Some extra tidbits: You may have noticed that for a "hide your shit" post, there isn't any mention of the tor project and there is a simple reason for this. You should never use bittorrent over tor....ever....because it wasn't made for bittorrent traffic, it just won't work and if you do it, your results WILL be subpar. Also, sending your traffic through these things will show a performance hit in torrenting speed, but at least you can't get rekt until they have a probable cause that you have amassed a huge amount of something and find your tomb on your confiscated computer and forcefully find a way to get the key out from your cold, dead hands. Unfortunately, this means that you need to manually handle your tomb file and optionally make a script to clean up your dirty work so only you know where it is.
