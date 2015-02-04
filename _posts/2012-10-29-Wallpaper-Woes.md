---
layout: post
title: Wallpaper Woes
---

So it has been much less than an actual week, but accomplishment deserves some sort of timely banter.

As it turns out, I'm a person with a preference towards a little bit of eye candy, and by little I actually mean a lot. Unfortunately, I've never been one for burning too much resources for just eye candy, removing the more fun stuff like compiz, but there was one particular feature that I really wanted to have on my fairly minimalist styled laptop (Archlinux+Openbox+Compton). That feature happens to be wallpapers.

Now putting just a single wallpaper for all desktops is fairly easy, but I'm a fickle person, so that won't satisfy me. Compiz happens to have the amazing ability to set different wallpapers for each desktop AND rotate them at a set interval (random rotation though). Rotating desktop wallpapers isn't super hard, and there are various scripts out there that could help. The one I happen to use gives an equal chance for all images to appear (even the current one), meaning that the same image could appear twice. It's a quirk I haven't dealt with, but it's something I can survive with anyways. As for different wallpapers per desktop, there are relatively less scripts out in the wild for whatever reason.

The one that I found was a python script, and it was relatively usable, though it did seem slow with the rapid desktop flicking that I regularly do for no reason. Also, I just happen to like bash more for doing stuff. From there, the journey that lasted a couple of hours to create what would probably be a relatively simple script for someone else who is deeper into bash scripting than myself (I consider myself a complete noob at scripting). After much toiling and poking, I finally made a script that works both as a wallpaper changer as well as a pipemenu for openbox. It's posted at the bottom of this post.

Supposedly the script should be called while one is using shortcut keys to move between desktops (I generally leave mine at cycling through all of my desktops). It would be nice if I could get it to run on every instance of the desktop change, but unfortunately I have no idea how to do that, so I'll just live without using my mouse to swap between my desktops :3

It should change to a random wallpaper within the defined folder when called without any arguments since it was simple and cooler than just having four desktop images.

Not only that, but by putting it in the openbox menu as a pipemenu (just use the script again, but add a "c" argument), you are able to change the desktop background for the current desktop (since I don't really know how to make a pipemenu, I had to keep it simplistic enough for myself to actually create).

It takes an equal number of directories to desktops which are easily definable at the beginning of the script, though you have to add the directories yourself and expand if needed.

To summarize what it does:
Changes desktop background to a random image in a defined folder specific to the virtual desktop and gives openbox users the option to change to a different image from within the folder.

So, if you happen to be like me and use IRSSI with a semi-transparent terminal, you can set a specific folder aside (wallpapers/dark for me) just for that full screen irssi you have. Why would I do that? Simply because I have wallpapers that range a variety of colors and some of them make it really hard to see my irssi chat, so I'll just keep the background a set sort of color, and my eyes don't have to strain to see the text.

Also, the command that I used to set the random image is searching for symlinks (I have the actual images in a different folder, and duplicates waste space), so keep that in mind.

-------------------------------------------------------------------------------------------------
 
    #!/bin/bash
    #--------------------------------------------------------------------
    # Wallpaper Changer Script
    # -------
    # Allows for separate image directories for each virtual desktop
    # and will change to random images within the directory when called.
    # Calling with "c" is for an Openbox pipemenu allowing the user to
    # manually change the current wallpaper to a different one within 
    # the defined desktop image directory.
    # Configurations
    # -------
    # Choose directory with images for each desktop
    # Change command of wallpaper setting program (feh default)
    # Also, default feh command searches for symlinks****
    #-------------------------------------------------------------------
    DIR0='/home/jplayer/.config/openbox/wallpapers/light'
    DIR1='/home/jplayer/.config/openbox/wallpapers/dark'
    DIR2='/home/jplayer/.config/openbox/wallpapers/cute'
    
    DESKTOP=$(xprop -root _NET_CURRENT_DESKTOP | awk '{print $3}')
    DIRNAME=DIR$(xprop -root _NET_CURRENT_DESKTOP | awk '{print $3}')
    eval DIR=\$DIRNAME
    if [ "$1" == "c" ]; then
        echo '<openbox_pipe_menu>'
        DESKTOP=$(($(xprop -root _NET_CURRENT_DESKTOP | awk '{print $3}')+3))
        DESKTOPID=$(xprop -root _NET_DESKTOP_NAMES | sed 's/"//g' | sed 's/,//g' | awk "{print \$DESKTOP}")
        echo '<separator label="'$DESKTOPID'"/>'
        echo '<item label="Random">'
        echo '<action name="Execute">'
        echo '<execute>~/.config/openbox/pipemenus/wallpaper</execute>'
        echo "</action></item>"
        # List files in corresponding desktop directory
        ls $DIR |
            (
            while read line
            do
                echo $line
                echo '<item label="'$line'">'
                echo '<action name="Execute">'
                FEH='feh --bg-fill '$DIR/$line
                echo '<execute>'$FEH'</execute>'
                echo "</action></item>"
                done
            )
        # Close PipeMenu
        echo '</openbox_pipe_menu>'
    # Handle wallpaper changing
    else
        feh --bg-fill $(find $DIR -type l -iregex '.*\.\(bmp\|gif\|jpg\|png\) | sort -R | head -1)
    fi

------------------------------------------------------------------------------------------------------------

To wrap up this post, I don't claim to be amazing at bash scripting and if anything, I am terrible at it. I have absolutely no idea whether this is the optimal way to get around or not, BUT if anyone knows how to make it better, or maybe have a completely different script, feel free to post it and let others know about it. Granted it might not get very far on this rather unknown blog, but hey, at least I'll know about it :3.
