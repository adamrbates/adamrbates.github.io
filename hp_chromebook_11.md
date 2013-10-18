---
layout: base
title: HP Chromebook 11
---

Installing ChrUbuntu on the new HP Chromebook 11
===

Shortly after recieving my HP Chromebook 11 I started to play around with ChrUbuntu. So far there isn't a lot of helpful info out there. This is a log of what I have found so far.

Optimistic start
---

Start by following the basic instructions on [Jay Lee's ChrUbuntu Site](http://chromeos-cr48.blogspot.com/2013/05/chrubuntu-one-script-to-rule-them-all_31.html).
Steps 1 through 7 should work properly. In step 8 use the following command,

    curl -L -O http://goo.gl/s9ryd; sudo bash s9ryd xubuntu-desktop lts

The add-apt-repository commands fail causing *xubuntu-desktop* not to install. **DO NOT REBOOT!** CTRL-C out of the script and chroot into the ChrUbuntu partition.

    chroot /tmp/urfs
  
Uncommented all the sources in */etc/apt/sources.list*. After editing the file it should look like this:

    # See http://help.ubuntu.com/community/UpgradeNotes for how to upgrade to
    # newer versions of the distribution.
    
    deb http://ports.ubuntu.com/ubuntu-ports/ precise main restricted
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise main restricted
    
    ## Major bug fix updates produced after the final release of the
    ## distribution.
    deb http://ports.ubuntu.com/ubuntu-ports/ precise-updates main restricted
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise-updates main restricted
  
    ## Uncomment the following two lines to add software from the 'universe'
    ## repository.
    ## N.B. software from this repository is ENTIRELY UNSUPPORTED by the Ubuntu
    ## team. Also, please note that software in universe WILL NOT receive any
    ## review or updates from the Ubuntu security team.
    deb http://ports.ubuntu.com/ubuntu-ports/ precise universe
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise universe
    deb http://ports.ubuntu.com/ubuntu-ports/ precise-updates universe
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise-updates universe
  
    ## N.B. software from this repository may not have been tested as
    ## extensively as that contained in the main release, although it includes
    ## newer versions of some applications which may provide useful features.
    ## Also, please note that software in backports WILL NOT receive any review
    ## or updates from the Ubuntu security team.
    deb http://ports.ubuntu.com/ubuntu-ports/ precise-backports main restricted
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise-backports main restricted
  
    deb http://ports.ubuntu.com/ubuntu-ports/ precise-security main restricted
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise-security main restricted
    deb http://ports.ubuntu.com/ubuntu-ports/ precise-security universe
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise-security universe
    deb http://ports.ubuntu.com/ubuntu-ports/ precise-security multiverse
    deb-src http://ports.ubuntu.com/ubuntu-ports/ precise-security multiverse
    
Now install xubuntu-desktop and some other helpful utilities.

    apt-get update
    apt-get install xubuntu-desktop
    apt-get install chromium-browser xfce4-battery-plugin xfce4-mount-plugin xfce4-volumed xfce4-panel xfce4-mixer
    

Exit the chroot by hiting CTRL-D or typing *exit*.

Before rebooting copy the pepper flash player plugin that chrome uses.

    mkdir -p /tmp/urfs/opt/google/chrome/pepper
    cp /opt/google/chrome/pepper/libpepflashplayer.so /tmp/urfs/opt/google/chrome/pepper
    
To get Chromium to use the Chrome flash plugin we need to add two flags to */tmp/urfs/etc/chromium-browser/default*. Log into your Chromebook to start the Chrome browser. Then CTRL+ALT+-> back to the console and type,

    ps ax | grep "\-\-ppapi-flash"

Look for the two flags `--ppapi-flash-path` and `--ppapi-flash-version`. This is what Chrome is using on my system,

    --ppapi-flash-path=/opt/google/chrome/pepper/libpepflashplayer.so --ppapi-flash-version=11.9.900.117-r1
    
Add these flags to the CHROMIUM_FLAGS variable in */tmp/urfs/etc/chromium-browser/default*. It should look something like this,

    # Default settings for chromium. This file is sourced by /usr/bin/chromium-browser
    #
    # Options to pass to chromium   
    CHROMIUM_FLAGS="--ppapi-flash-path=/opt/google/chrome/pepper/libpepflashplayer.so --ppapi-flash-version=11.9.900.117-r1"
    
It is time to reboot.

    sudo reboot
    
You should have a working xubuntu system.

Track Pad Issues
---

From [here](http://www.eddiedillon.info/?p=477) I found some helpful commands to fix the trackpad. The trackpad on the HP is called "Atmel maXTouch Touchpad" so replacing "Cypress APA Trackpad (cyapa)" with the correct trackpad name is all we need to do.

    xinput set-prop "Atmel maXTouch Touchpad" "Synaptics Finger" 15 20 256
    xinput set-prop –type=int –format=8 "Atmel maXTouch Touchpad" "Synaptics Two-Finger Scrolling" 1 1
