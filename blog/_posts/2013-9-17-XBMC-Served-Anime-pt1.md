---
layout: post
title: XBMC Served Anime pt.1
tags: [Anime, Application, Eyecandy, Linux, Management, Video, Windows, XBMC]
---

There are pretty much three ways to get XBMC and anime to work together in a way that actually uses XBMC's ability to scrape websites like TVDB or IMDb to present your anime with beautiful eye candy.

Path of Renaming: Manually go and rename ALL of your anime to comply with the standards used in XBMC.

Path of Convoluted Thought: Utilizing regular expressions to recognize your anime so scraping works and thus, possibly the least amount of thought AFTER figuring out the perfect universal way to recognize any anime from any series by any fansub group despite how they might attempt to name the file.

Path of Hardcore Manual: Possibly the LEAST productive way to go about dealing with your anime by adding a .nfo file for EVERY SINGLE EPISODE of EVERY SINGLE ANIME that you have.

In-depth Renaming:
So renaming isn't quite the most amazing way to do things, but it's currently my chosen method of tackling the problem and of course, I'm not alone in my plight. Thus comes the issue of how exactly are anime lovers supposed to rename their forever expanding archive of anime with the least amount of hassle?
For this problem, batch renaming is obviously the key to solving the issue and so here comes some file renaming programs that may or may not suit your fancy.
- [Anime Batch Rename](http://sourceforge.net/projects/animerename/) from jigglyslime
- [AnimeBatchRename](https://github.com/Deathspike/AnimeBatchRename) from Deathspike
- FileBot (http://www.filebot.net/)

Now the above list is in no way exhaustive and there are multitudes of other batch renaming programs available, or there is also scripts and such that can be used (though the same issue occurs as the path of convoluted thought). I currently use the renamer from Deathspike (Windows only) because I just happened to like how it looks and there was an optional registry edit to add the program to the right click context menu, but to be honest, FileBot really looks to be the way to go. It's the most feature-full of the three programs and is meant to be used in conjunction with a media center (and of course, there are versions for Windows, Mac, and Linux).
The only real drawback is that the fansub group has to be added to their database before it can be detected by the program (so currently EveSenshi doesn't work). The program from Deathspike is a little more concrete in its formatting so you can't really rearrange anything, but at least you should be able to trust information you add in personally.

In-Depth Convoluted Thought:
So this is kind of the issue with importing anime, there isn't like a set defining way to label anime releases. Some of them have the resolution, others don't, or some are released as joint releases and others not, but still utilizing subs from other groups (like re-encodes). It's almost a mess as it is without counting the retarded titles of some of the sequel anime series like...Dog Days'...K-On!!...or even Oreimo.  (yes, the period designates the second season). And then there is stuff like Black Rock Shooter that actually have a fucking star in their title. All that stuff kind of has to be taken into account and then you supposedly make a batch script or bash script, etc. in order to automate the process for you. In my opinion, if there were an all-encompassing script that could take all of that into account and then you choose to toggle on or off certain bits of information, it would be the superior option...but otherwise, I'd rather like to just go through and make sure things are correct the first time around without having to go back and rename the odd ones. I think XBMC also allows for tv show matching...but that doesn't change the main issue above.

In-Depth Hardcore Manual:
You would have to be either fucking insane or fucking dedicated to want to do something like this. This would literally mean pretty much doing the scraping manually for every episode and every anime which defeats the whole purpose...though if you export your database via XBMC after importing everything, a couple of tweaks to specific .nfo files that strike your fancy is always an option.
