---
layout: post
title: The PSP Firmware
tags: [Blurb, CFW, Game, ME/LME_cfw, Playstation, PRO_cfw, PSP]
---

So the topic of the day is PSP custom firmware, specifically for firmware version 6.60 since most official updates are kind of just for online stuff and some of the newer games. Over the years, there has been a fair number of custom firmware like M33, HEN, PRO, ME/LME, etc. that seem to come around every major firmware release. The two main custom firmware as of now are PROcfw and ME/LME, both of which saw releases around the end of 2014 to early 2015.

Between the two custom firmware, there are a variety of features added beyond the official firmware. These include playing backup isos/csos/etc, homebrew, psp plugins, PS1 emulator (POPS), changing CPU speed, hiding the MAC address, a VSH menu, recovery menu, registry hacks, and other things.

PRO-C2 (Feb-14-2015):
PRO cfw recently added zso support, which uses lz4 compression and is somewhat better than cso, but next to nobody uses it and since most of the tools aren't really updated, there is only maxcso that can easily convert to the format. Honestly I have very little to say about PRO cfw. It does what it's supposed to and for the most part, things just "work" whereas sometimes ME/LME requires an extra plugin (just thinking about POPS in particular). With that said, plugins seem to be handled slightly differently between PRO and ME, so if you are really aiming for a certain set of plugins that for some odd reason don't seem to work, trying out ME/LME is an option.

ME/LME (Jan-23-2015):
Just as PRO added zso, ME/LME added dax support, which seems to be currently the best format for compressed isos. In my case, I prefer to use the ME/LME cfw primarily because it allows me to run both System Menu and XMBCtrl plugins at the same time without completely breaking USB functionality (sort of). ME/LME also has a couple of more advanced features that are required for the common PSP user. These features include formatting Flash1/2, executing OPNSSMP.bin/PBOOT.PBP, and (only available for PSP1000) softmodding the battery to a Pandora/Autoboot/Normal battery. The inferno iso loader was also added to ME/LME rather recently and seems to be the best out of all of the available iso loaders (in my short tests). Time Machine and the leda.prx plugin which allows for kernel 1.50 emulation (for old homebrew) are supported if you are really hellbent on PSP homebrew and older cfw/ofw.