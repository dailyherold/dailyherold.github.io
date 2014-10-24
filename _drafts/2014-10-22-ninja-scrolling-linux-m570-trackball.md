---
layout: post
title: Scroll Like A Ninja - Logitech M570 Trackball + Linux Mint
excerpt: Configure your M570's trackball to emulate the scroll wheel
date: 2014-10-22 00:00:00 
author: John Paul Herold
categories: linux trackball m570
published: true
---
### This is how I configured my Logitech M570's trackball to act as a "scrollball"
***
David, a co-worker of mine, was describing his *somewhat* affinity for the Apple Magic Mouse. Myself with an anti-Apple bias, and pro-trackball bias, attempted to shoot down any and all points. He got me here though, "I like just being able to scroll in any direction with touch." 

"That would be nice," I thought. We code, we don't have a strict coding guidline, and horizontal scrolling extends far beyond 80px in many/all cases. Plus I am always happy on the Y-axis with my scrollwheel, but any x-axis movement still requires me to find the scroll bar and do it the old-school way. Cue Google rabbit hole...

1. Google: [M570 Horizontal scrolling](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&es_th=1&ie=UTF-8#q=m570%20horizontal%20scrolling&qscrl=1) -> [1st result](http://forums.logitech.com/t5/Mice-and-Pointing-Devices/M570-Horizontal-Scrolling/td-p/656358)
  * _"People are thinking the same thing I am"_
2. Google: [Universal scroll trackball](https://www.google.com/webhp?sourceid=chrome-instant&ion=1&espv=2&es_th=1&ie=UTF-8#qscrl=1&q=universal+scroll+trackball) -> [1st result](http://forums.logitech.com/t5/Mice-and-Pointing-Devices/universal-scroll-on-marble-mouse/td-p/161730)
  * _"A solution appears to exist, please Linux support...."_
3. Google: [Marble mouse scroll wheel](https://www.google.com/search?q=Marble+Mouse+Scroll+Wheel&oq=Marble+Mouse+Scroll+Wheel&aqs=chrome..69i57j69i60&sourceid=chrome&es_sm=93&ie=UTF-8&qscrl=1) -> [2nd result](https://wiki.archlinux.org/index.php/Logitech_Marble_Mouse)
  * _"YES, ArchLinux documentation"_

At this point I know a solution exists, although it seems all the guides are for the "Marble Mouse", Logitech's ambidextrous trackball:

![Logitech Marble Mouse]({{ site.url }}/assets/marbleMouse.png)

Which is great, and no offense against left-handers, but I think my Logitech M570 is the greatest [non-gaming] mouse in the world:

![Logitech M570]({{ site.url }}/assets/m570.jpg)

- Wireless (with great battery life)
- Insanely comfortable 
- Precise
- Easy to clean trackball 


Research
- google: linux mint scroll modifier
- http://forums.linuxmint.com/viewtopic.php?f=49&t=87176
- troubleshooting section of https://help.ubuntu.com/community/Logitech_Marblemouse_USB