---
layout: post
title: The PSP Archive
tags: [Archiving, Blurb, Game, Playstation, PSP]
---

So when talking about archiving things for the PSP, the main problem lies in how large PSP games are (they range from ~40MiB to ~2.5GiB). There are a couple archive formats that PSP custom firmware can use without any major issues most of the time, but at the moment the best standard is up in the air.

- iso format: The uncompressed release. All cfw should be able to use it just fine, but it tosses all file space to the wind.
- cso format: The defacto standard for the PSP since forever. Similar with iso format, all cfw should be able to use it just fine.
- jso format: A rather unknown and not exactly amazing format that next to nobody uses. It is worse than cso in terms of compression ratio.
- zso format: A fairly recent addition to the PRO cfw (added Dec 2014) that is supposedly slightly better than cso in both compression and usage speed.
- dax format: Just as zso is for PRO cfw, dax support was added to ME/LME cfw in Dec 2014. Similarly, faster and smaller than cso. Has the best compression ratio out of the formats above.

Dax format seems to be the best, but it's only been added to the ME/LME cfw and so PRO cfw users are somewhat out of luck there. Having just an archive of dax files is fine, but I find that converting to dax format is not particularly hard or tedious to me, thus I tend to be slightly more zealous in saving file space. When archiving, 7zipped iso files has the highest file compression in comparison to 7zipping any of the PSP compression formats (compressing compressed files never seems to work), and is my method of choice.

Now with all of the compression out of the way, there are other things that can be done to squeeze out some more file space, but pretty much requires UMDgen and may not produce working isos. Within each PSP iso, there is an UPDATE folder that has some files that, more often than not, are completely useless to the normal PSP user. UMDgen has both an "optimize" and "find dummy files" functions to resize those supposedly unneeded files to small dummy files, generally saving ~30-40MiBs per iso. Out of the 45-ish isos I tested, 9 games failed to run after optimizing, so it's probably best to give a quick check before archiving "optimized" isos.

Optimizing each iso ends up being really tedious to extract a clean iso from my archive, optimizing, saving new dax, testing, rearchiving. All in all, it saves very little (32 different games optimized saved a little under 1GiB), and so opting out of optimizing isn't always a bad idea. Of course, some games might have some substantial savings, while others don't. So when you have hope, have at it, otherwise you don't have to fret too much  because you aren't saving enough to really fit another 3-4 games or anything anyways.
