---
layout: post
title: Raspberry Pi from Two
tags: [Archiving, File_System, Linux, Management, Raspberry_Pi, Setup]
---

So continuing the thread of the raspberry pi, we currently have some magically way of managing our raspberry pi via ssh.

- A storage place for the stuff we are hoping to use our torrent client for
- RSS aggregator of some sort to sift through the torrents we are looking for and grab the ones we want
- A torrent client to actually get them without killing our pi
- A protocol to serve our media to another device on the network
- Maybe something to hide ourselves so we aren't blatantly telling the world that anime is all we ever want

So now things come to how we are going to store our data and because of how completely dense this topic is, handing out a singular solution would probably not help at all. Fortunately, there are some things that could be solidified even if you decide to pick and choose some other bits and pieces.

First off is the storage medium, which is to say the hard drive, usb stick, ssd, etc. that we are planning to store all of our anime on whether temporary or otherwise. While probably the easiest, it is also probably the slowest to use the space on the SD card thanks to low read/write speeds, thus we definitely don't want to use that for storage. Usb sticks would probably fare somewhat poorly as well since their wear and tear is also not the greatest considering we are constantly writing and reading from it (supposedly, though technically it's only really maybe one-two days per week when all the episodes are released). So of course, the best choices would be an external hard drive (spinning or ssd), both of which are decently fast and are pretty cheap now for large sizes.

With our choice of storage picked, we need to get a type of file system to deal with all of our stuff. This is where things are really user opinion because really it's not like any of the file systems are bad or anything, but we will do a quick rundown anyways.

- Fat32: Probably one of the few that should be tossed out of the list. Sure you probably won't hit many 4GiB+ files, but you never know when you want that 4K FLAC movie that is like 50GiBs.
- ExFat: It's kind of inbetween NTFS and Fat32. It allows for large files and has a smaller footprint than NTFS...but it's really only for flash memory, so maybe on an SSD at best.
- NTFS: The defacto standard for Windows and thus pretty much all hard drives will end up being formatted to it. It's not bad, and if you have a Windows computer, it could be more convenient...though you might need to defrag it eventually...
- HFS/HFS+/Mac stuff: No.
- ZFS/BtrFS/XFS/JFS/etc: This is where stuff becomes just a toss-up. If you know the differences between all of them, then chances are you don't need this post to tell you anything.
- Ext3/4: The defacto linux standard for the time being. Probably a decent pick normally and defragging isn't something you have to worry about, but at this point, you might want to just ditch Windows entirely because while you can use it with Windows, it's not perfect for whatever reasons.

As an extra note, if you are planning on having multiple hard drives attached or something, it doesn't hurt to look at things like btrfs or xfs or other things. Most of that really comes to preference and what you trust. Since I know and trust ext4, that WOULD be what I would love to format my hard drive to...but I started my collection long ago and thus I've got to stick with NTFS for my current hard drive (converting at this point would take ages with my almost full 2TB hard drive). There is also possibly a larger overhead when using NTFS rather than ext3/4 that could also slow down our limited raspberry pi.

So we now have a great hard drive to put our stuff in, and hopefully a good file system to go along with it (I won't hate you if you stick with NTFS, but I will hate you if you decide on HFS+ or something). Now we need a way to get a list of the torrents that we want, preferably from an RSS feed because those are still around.
