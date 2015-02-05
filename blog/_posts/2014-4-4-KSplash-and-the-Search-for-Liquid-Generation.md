---
layout: post
title: KSplash and the Search for Liquid Generation
tags: [Anime, Application, Art, Bash, Blurb, Eyecandy, KDE, KSplash, Linux, QT]
---

For the past couple of days, I've been living in a KDE desktop. Some would say this is insane, others would say that I made the right choice and to be honest, I haven't checked out much of the actual environment to make a judgement call. In terms of what I installed, I went for a minimal kde setup to give me the settings and desktop to play around with.

For most of my times in linux I always seem to try to aim for a sweetspot between lightweight minimalism and eyecandy. I originally thought that I might try the standalone compiz again, but the dying momentum behind the project did make me wary and of course I'm one of those people who like to see progress. Thus, I turned towards the supposed equally good kwin to provide the eyecandy I'm looking for while hoping to also hit the minimalistic theme that I want. This also will me a good chance to try out some of the QT apps in all of their glory rather than in a GTK environment.

Of course with everything that I do, I get side tracked on the strangest of uselessness and ended up wanted to make my own ksplash screen in hopes of at least loving my splash screen if I can't remove it. Upon a couple of quick searches, I ended up finding KSplasherX and KSplashQML with the former being an actual application to make simple themes for ksplash and the latter being the second engine of ksplash that utilizes QML to define how things are displayed. Without knowing QML and not wanting to put in the effort to learn it (though in actuality, it doesn't look very hard at all to learn), I tried out KSplasherX only to find it rather limiting in what it could do. In the end, I sifted through the few files of a couple of themes that used the KSplashX engine and slowly pieced together what most of the needed parts. Surprisingly enough, I couldn't find any real detailed information about how to make a KSplash screen from scratch, but it isn't like I tried hard.

So on to the main reason for posting: giving the stuff in the KSplash theme some layman's terms to describe all of it for my own memory.

The local themes are installed into ~/.kde/share/apps/ksplash/Themes and are separated into their own folders. Each theme has the variety of resolutions the theme is available in, a single preview image that seems to fall between 300~500 x 200~300px, and a theme.rc. The theme.rc is just a simple text file defining some of the metadata-like stuff (author, homepage, version, name, description) and the engine that the theme will use (KSplashX or KSplashQML as far as I know for KDE4). A QML theme will also have a main.qml that is the actual theme-ing part of the QML theme, but for KSplashX themes, those are inside of the resolution folders.

The contents inside each resolution folder is pretty basic. It consists of a singular description.txt (which could be considered the equivalent of main.qml) and the images that are used in the theme. Animated images need to be placed inside of a single image and aligned in a grid going from left to right, top to bottom, with each row having a maximum of 10 frames of the animation. Because fuck it, here is an example image.
![Example Image](http://2.bp.blogspot.com/-M9AbthWxbFc/Uz5lsPs5CLI/AAAAAAAAAiw/tIkcWyVgC4k/s1600/dance00.png)

As for the file names of these images, they can literally be almost anything you want, but you are going to need to type those into the description.txt so shorter is nicer...probably. Which brings up the next section of this post: the description.txt.

The meat of your KSplashX theme, this file tells what pictures to show, in what order, where to show them, etc. So, tossing formatting to the wind, I'll just list this stuff out.
- SCALE : It seems to have the option of "ON" or "OFF". For better or worse, I'm leaving it on since it seems to be what everyone else does.
- SCALEX : I can only assume it has something to do with scaling in the X axis (option of "ON" and probably "OFF").
- SCALEY : Likewise for the Y axis.
- BACKGROUND_IMAGE : Two numbers proceeding the image name, though what those two numbers actually do, I have no idea.
- WAIT_STATE : There seems to be around 7 states (that I found so far) during the KDE splashscreen that you can use as checkpoints...though whether the first one can really be considered is if you think the starting line is a checkpoint (initial, kded, kcminit, ksmserver, wm, desktop, ready).
- ANIM_REL : Defines which animation it is, where to place it, what image, how many frames, how many times to loop, and the delay between each frame. A little bit of expanding later on for this one, so hold on to your butts.
- IMAGE_REL : Similar to the ANIM_REL, but this is for static images rather than animated ones.
- STOP_ANIM : As one could expect, it stops the image defined.

So a quick semi-deeper look at ANIM_REL and IMAGE_REL, which place an animated and static image respectively in the order described in the description.txt.
Examples:
ANIM_REL 1 CC -320 200 CC 50 animation.png 30 20
IMAGE_REL CC -50 100 CC image.png

In the ANIM_REL, the first number represents what animation it is defined as so that, if needed, you can stop the animation later on using STOP_ANIM #. Next is "CC" which just seems to be about the equivalent of a separator...though I would like to think there is some secret function to them that I just don't know yet. Afterwards it's the offset of the image from the center of the screen X and Y respectively (negative towards the left and top, positive towards the right and bottom) followed by a second separator. This next number is the total number of frames in your animation (which in the example animation above, is 41) and then the actual image, followed up by the delay between each frame in...milliseconds for all I know. Finally, the 20 is the number of times to loop through the animation and can be omitted to have the animation loop until stopped or the splashscreen finishes. IMAGE_REL is set up exactly the same way, but removes the way to call back on the image (the 1 in ANIM_REL) as well as the number of frames and delay between each frame, since obviously a static image doesn't need them.

Some quirks to note about using transparency in the splash screen.
- Animated pngs WILL crop each other when overlapped, cutting the previous image with the newer one, regardless if the newer one has that section transparent or not.
- Placing a static image before starting an animation or another transparent image will result in the expected result of having the static image included in the transparency of the newer image.
- Images after animated pngs overlapped, but before stopping the animation will result in the animated image having a higher priority than the static image and cut into the static image.
Images overlapping after animated pngs are stopped will crop into the now stopped animated png instead of incorporating the now static animation into the transparency.
- You can start or place multiple images/animations almost anywhere inbetween each loading state and have them appear at roughly the same time.

I've also noticed that on my computer, if I stop the animations before a certain point, the stopped animations seem to vanish completely for some unknown reason. As an extra tidbit though, a DoctorWho themed KSplash that even included a sound file to go along with it. Usage apparently consisted of just putting a bash script (don't forget the .sh at the end...maybe?) in the ~/.kde/env folder that used aplay/mpg123 to play a sound file somewhere on the filesystem.

And with that ends my current adventures with KSplash. As a show of the testing product I ended up with while playing around (not what I plan to eventually have as my splash screen, but a decent placeholder).
![Test KSplashX](http://i.imgur.com/ux4anuE.png)
