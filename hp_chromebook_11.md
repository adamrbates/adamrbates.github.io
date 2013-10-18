---
layout: base
title: HP Chromebook 11
---

Installing ChrUbuntu on the new HP Chromebook 11
===

Shortly after recieving my HP Chromebook 11 I started to play around with ChrUbuntu. So far there isn't a lot of helpful info out there. This is a log of what I have found so far.

Optimistic start
---

First, I started by following the basic instructions on [Jay Lee's ChrUbuntu Site](http://chromeos-cr48.blogspot.com/2013/05/chrubuntu-one-script-to-rule-them-all_31.html).

Track Pad Issues
---

From [here](http://www.eddiedillon.info/?p=477) I found some helpful commands to fix the trackpad. The trackpad on the HP is called "Atmel maXTouch Touchpad" so replacing "Cypress APA Trackpad (cyapa)" with the correct trackpad name is all we need to do.

  xinput set-prop "Atmel maXTouch Touchpad" "Synaptics Finger" 15 20 256
  xinput set-prop –type=int –format=8 "Atmel maXTouch Touchpad" "Synaptics Two-Finger Scrolling" 1 1
