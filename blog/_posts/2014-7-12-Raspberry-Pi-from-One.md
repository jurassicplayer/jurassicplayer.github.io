---
layout: post
title: Raspberry Pi from One
tags: [Application, Archlinux, Management, Raspberry_Pi, Setup, SSH, Static IP]
---

So we finally have a wonderfully set up raspberry pi that is ready to have a bazillion packages installed to bloat it. First, we need a list of the things we'll need in order to turn this pi into a media server that better be hauling weights for us.
From start to finish:

- Some way to manage our raspberry pi if we want to update or something goes wrong
- A storage place for the stuff we are hoping to use our torrent client for
- RSS aggregator of some sort to sift through the torrents we are looking for and grab the ones we want
- A torrent client to actually get them without killing our pi
- A protocol to serve our media to another device on the network
- Maybe something to hide ourselves so we aren't blatantly telling the world that anime is all we ever want


For the first point, we are going to need to have some way of managing the raspberry pi without needing to actually plug it into a monitor, which takes effort and another keyboard, etc. While you could go with a remote desktop sort of thing, that is really inefficient and wastes resources and so we are going with the best of basics and stick with ssh. Thankfully, openssh is installed by default so there really isn't much we need to do to the raspberry pi except get the IP address somehow. Of course, we could always just use "ifconfig" if we have every cumbersome thing plugged into the raspberry pi, but that doesn't really help much unless you set the raspberry pi to use a static IP. So we are going to set the raspberry pi to use a static IP and make things easier on ourselves. How is this easier? Well that is because otherwise, you would need to be able to find the IP address of your raspberry pi from some other device (which IS possible, but somewhat inconvenient) or have your raspberry pi with a DNS service to resolve a custom domain into your raspberry pi's IP address (also somewhat inconvenient).

Setting a static IP:
To set up a static IP on your raspberry pi is actually pretty easy thanks to netctl (remember this is Archlinux, I'm not going to care about other distros). Simply "cd /etc/netctl/examples", check what examples match your setup (ethernet-static, wireless-wpa-static, etc.) and copy that into the parent directory with "sudo cp ethernet-static ..". A little bit of editing is needed, so we will need to use "sudo nano ../ethernet-static" and it should say 

    Description='A basic static ethernet connection'
    Interface=eth0
    Connection=ethernet
    AutoWired=yes
    IP=static
    Address=('192.168.1.23/24')
    Gateway=('192.168.1.1')
    DNS=('192.168.1.1')

Make sure to take note that there is an extra line that says "AutoWired=yes" that needs to be added. You can also change the IP address that is going to be used by changing the line "Address=('192.168.1.XX/24')" to another IP address that you can remember. Also just to make sure, add parenthesis around the "Gateway" value so things don't go awry.

Almost done, just need to enable the configuration by typing "sudo netctl enable ethernet-static" and your raspberry pi will try to get that IP address automagically after booting. At this point, we can now ssh into our newly static raspberry pi and ditch the cumbersome extra equipment (screen, keyboard, etc.), so feel free to shutdown the raspberry pi and reboot it after placing it somewhere hidden, like under the bed. You can connect to your raspberry pi by using an ssh client (there are quite a few out there) as long as you have your username, password, and the IP address of your raspberry pi. Generally the format is "username@IPaddress" and then after connecting, you are asked if you trust the computer and then you can type in your password to log in.

From here on, everything that I'm doing is going to be via ssh, not that there is really going to be a difference. Next is to look at the different aggregators that we can use and hopefully pick one to stick with.
