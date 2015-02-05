---
layout: post
title: Android and LibMTP
tags: [Android, Archlinux, File System, gvfs, libMTP, Linux, Management, MTP, polkit]
---

So obviously being a cool guy, you have an android device or two that you may or may not want to connect via usb to your computer. While in windows, mtp is either barely passable or you just can't get the shit to work at all. In both cases, mtp is annoying to use and all around terrible in comparison to the old way of just mounting directly in my opinion. Fortunately, the Archlinux wiki has an article on that (as with almost everything else ._.) and all that really needs to be installed are gvfs and libmtp, though apparently gvfs-mtp is still available as a separate package. It has already been merged upstream and released with gvfs 1.15.2, so any version after, you probably don't need gvfs-mtp.

As a quick note, installing just gvfs and libmtp on a vanilla Arch install as of right now won't give you a working auto-magically mounting android MTP device. This is because gvfs apparently also needs a polkit authentication agent like pkttyagent, lxpolkit, mate-polkit, polkit-gnome, or polkit-kde...and that needs the session to be started with dbus-launch. Pkttyagent is included by default as a console auth agent, though I use lxpolkit anyway since having a GUI agent allows me to open things like GParted with pkexec.

Usually this is enough to get things working, but sometimes the device might have not been added yet to their massive list of udev rules. Of course instructions are on the wiki so you can just add those, reload udev rules, and maybe reboot.

Now this still has the same problems that mtp does on Windows, primarily that what is shown on the computer and the phone do not always show the same things after manipulating files with the phone because that is how MTP supposedly works. Anyway, with that MTP should supposedly be working and all sorts of magic happening with any file manager that supports gvfs (PcManFM, Thunar, etc.).
