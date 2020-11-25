---
layout: post
title: Raspberry Pi from Four
tags: [Application, Archlinux, FTP, FTPS, Linux, Management, NFS, Raspberry_Pi, rsync, Samba, scp, Setup, SFTP, SSH, SSHFS, Streaming, UPnP/DLNA, Video]
---

So we are finally complete with all of our wondrous automation on our raspberry pi and thus, now is the time for a recap of what remaining stuff we need to deal with.

A protocol to serve our media to another device on the network
Maybe something to hide ourselves so we aren't blatantly telling the world that anime is all we ever want

Today is a day where we will talk about another subject that I know next to nothing about, but that is totally fine because the internet never lies. Only truths here... Anyways, we are just taking a quick poke at a couple of major types of file transfer protocols so we can stream stuff...maybe...Mostly, it's a preference thing and word from other people about the general performance differences between the different protocols, so I can't hate you for any choice you decide to go with. Of course with that said, it's probably best to get the lightest and fastest because the raspberry pi has a single controller for the ethernet and usb ports, thus it's already shoveling stuff through a pipe as it is. 

- FTP: It's better than SMB at least. Worst case, it's always worth it to give ftp a shot if all else fails.
- FTPS: The more preferred way of ftp usage because we need to be secure about our anime. 
- SMB: Normally a pick if you have a windows computer, but you don't because I say so. Also, it's not the fastest out there, therefore SMB isn't needed.
- SSHFS: This should really only be compared to NFS, and unfortunately NFS wins in the speed department.
- SFTP: Kind of follows suit with SSHFS, though it's not really surprising since it is also on top of SSH.
- SSH: A really clunky way to go about streaming (ssh user@ip "cat path/to/video" | mpv), but it's available if shit really hits the fan or you just don't want to use anything else.
- UPnP/DLNA: Literally only for streaming media and nothing else. No file manipulation or anything, so you would have to ssh into the pi to manage stuff. Not particularly bad, but being able to manage would be nice.
- RSync: Not really streaming so much as just copying it to your client computer, but I guess if you sent it to /tmp/ or something it could be considered streaming.
- NFS: Probably the best choice out of the group in terms of flexibility and performance, and the overhead isn't all that bad either.

Now, I haven't done any testing on the performance of ANY of these really. At best, I have tried nfs, ftp, and UPnP/DLNA, and in the end I preferred the nfs route. Thus, we are going to set up an nfs server.

First things first is to grab the nfs-utils package for both the server and client and make a new folder to hold all of our stuff that we are going to serve out. A simple "sudo mkdir -p /srv/nfs4/foldername" would do just fine unless you plan to split up your stuff and mount each to separate folders (ex. Music, Anime, Manga, etc.). Then we need to bind the mount point of our hard drive to the serving folder, this can be done manually with "sudo mount --bind /harddrive/path /srv/nfs4/foldername" or by adding another entry to the fstab to mount for you at boot that follows the lines of "/harddrive/path /srv/nfs4/foldername none bind 0 0". Then we need to set up what to export aka serve.

Opening the /etc/exports file, we want to write down what we want to serve, who we want to serve, etc. This should be pretty easy to do (even has examples) so something along the lines of "/srv/nfs4/foldername  192.168.1.0/24(rw,async,no_subtree_check,nohide)" will work just fine. Supposedly async and no_subtree_check will make the transfers just a tad faster, which is always nice. After that, "exportfs -rav" should update the exports and you should be ready to start the services with systemd. I would suggest enabling the services and then rebooting rather than just starting, but either is fine. Just "sudo systemctl enable nfs-server" should start up everything needed on the server.

On our client linux box, we need to set up our nfs client which can be done by enabling rpcbind.service and nfs-client.target. To check our setup, you can use "showmount -e" or "showmount -e raspberrypiIP" depending on if you are on the server or client respectively and to mount the nfs share client-side is a simple "sudo mount rPiIP:/srv/nfs4/sda1 /path/to/mount/point".

Supposing everything is working, we can optionally continue on and discuss about mounting automatically using fstab, systemd, autofs, etc. In my opinion, the "Tip and Tricks" section of the NFS page in the Arch wiki has a rather nice "[Automatic mount handling](https://wiki.archlinux.org/index.php/NFS#Automatic_mount_handling)" section that would probably work well in the event that our raspberry pi goes down for some reason (which might happen).

Once mounted, you should be able to read/write to it per usual and magical greatness should ensue. Some things to note:
- You can tune the performance by modifying the rsize and wsize options of the client-side mount.
- Streaming from NFS works well with animated shorts, but the same can't be unanimously said for large files. It may take a short amount of time to cache some of the video before you can have a smooth playing experience. Of course, this can be somewhat mitigated by tuning the performance.
