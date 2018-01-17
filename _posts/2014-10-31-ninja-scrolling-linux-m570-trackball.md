---
title: Scroll Like A Ninja - Logitech M570 Trackball & Linux Mint
excerpt: Configure your M570's trackball to emulate the scroll wheel
date: 2014-10-31 01:00:00
author: John Paul Herold
tags: linux trackball m570
published: true
---
### This is how I configured my Logitech M570's trackball to act as a "scrollball"
***
David, a co-worker of mine, was describing his *somewhat* affinity for the Apple Magic Mouse. Myself with an anti-Apple bias, and pro-trackball bias, attempted to shoot down any and all points. He got me here though, "I like just being able to scroll in any direction with touch."

"That would be nice," I thought. We code, we don't have a strict coding guidline, and horizontal scrolling extends far beyond 80px in many/all cases. Plus I am always happy on the Y-axis with my scrollwheel, but any x-axis movement still requires me to find the scroll bar and do it the old-school way. Cue Google rabbit hole...

1. Google 'M570 Horizontal scrolling' -> [1st result][1]
  * "People are thinking the same thing I am"
2. Google 'Universal scroll trackball' -> [1st result][2]
  * "A solution appears to exist, please Linux support...."
3. Google 'Marble mouse scroll wheel' -> [2nd result][3]
  * "YES, ArchLinux documentation"

At this point I know a solution exists, although it seems all the guides are for the "Marble Mouse", Logitech's ambidextrous trackball:

![Logitech Marble Mouse]({{ site.url }}/assets/images/marbleMouse.png)

Which is great, and no offense against left-handers, but I think my Logitech M570 is the greatest [non-gaming] mouse in the world:

![Logitech M570]({{ site.url }}/assets/images/m570.jpg)

- Wireless (with great battery life)
- Insanely comfortable
- Precise
- Great on any surface (desk, couch armrest, and bed included)
- Easy to clean trackball
- Looks nerdy

...enough with the sales pitch. Within a couple reads of the Arch Linux Marble Mouse [guide](https://wiki.archlinux.org/index.php/Logitech_Marble_Mouse), I figured that `EmulateWheel` and `EmulateWheelButton` setting from `/usr/share/X11/xorg.conf.d/50-vmmouse.conf` are the keys to my ninja scrolling success. Which button you assign to the `EmulateWheelButton` is up to you, however both the guide and I agree it would make sense to assign the setting to a middle mouse button (i.e. scroll wheel press). This setting is termed a "scroll modifier" which when held, like a modifier key, would allow you to roll the trackball in any direction for multi-axis scrolling goodness.

The guide emulates a middle click for the "Marble Mouse" with a simultaneous Right/Left click since the mouse doesn't have a scroll wheel. The M570 has a scroll wheel, so this emulation was not needed, and with some testing I confirmed that `2` is the correct button mapping for a middle click.

My initial config file had the following:

{% highlight bash %}
Section "InputClass"
	Identifier      "vmmouse"
	MatchIsPointer  "on"
	MatchTag        "vmmouse"
	Driver          "vmmouse"
EndSection
{% endhighlight %}

I added the necessary settings to the default "InputClass" block, and with great anticipation I restarted my X server (I just log out and in). Result? No change. Time to debug...

Luckily, I noticed at the bottom of the ArchLinux guide a reference to a [Ubuntu community guide][4] which covered mostly the same information apart from a very helpful 'Troubleshooting' section.

By tailing `/var/log/Xorg.0.log` and unplugging/re-plugging the Logitech wireless fob, I deducted some key information that, along with the rest of the guides, led me to modify my config file like so:

{% highlight bash %}
Section "InputClass"
    Identifier      "vmmouse"
    MatchIsPointer  "on"
    MatchTag        "vmmouse"
    Driver          "vmmouse"
EndSection

Section "InputClass"
    # User-defined name for this profile/input class
    Identifier      "Logitech M570"
    # Tailed /var/log/Xorg.0.log to figure out the following
    MatchProduct    "Logitech Unifying Device"
    MatchIsPointer  "on"
    MatchDevicePath "/dev/input/event*"
    Driver          "evdev"
    ## OPTIONS
    Option "SendCoreEvents" "true"
    # EmulateWheel refers to emulating a mouse wheel using the trackball
    Option "EmulateWheel" "true"
    # Set to middle-click
    Option "EmulateWheelButton" "2"
    # Affects distance trackball needs to move register scroll movement 
    Option "EmulateWheelInertia" "10"
    # Timeout between EmulateWheelButton click and "emulation" to begin
    Option "EmulateWheelTimeout" "200"
    # Comment out XAxis if you don't want horizontal scroll
    Option "ZAxisMapping" "4 5"
    Option "XAxisMapping" "6 7"
EndSection
{% endhighlight %}

After restarting my X server, and firing up [Sublime](http://www.sublimetext.com/), I CAN SCROLL LIKE A NINJA. Hope this helps the other ~0.067% of computer users out there who use a Logitech M570 with Linux. 


Happy Halloween,  
John Paul

***

####*Environment*
{% highlight sh %}
$ uname -a
3.13.0-24-generic #47-Ubuntu SMP x86_64 x86_64 x86_64 GNU/Linux
{% endhighlight %}

{% highlight sh %}
$ cat /etc/*-release
DISTRIB_ID=LinuxMint
DISTRIB_RELEASE=17
DISTRIB_CODENAME=qiana
DISTRIB_DESCRIPTION="Linux Mint 17 Qiana"
NAME="Ubuntu"
VERSION="14.04.1 LTS, Trusty Tahr"
{% endhighlight %}

***

####*External References*
1. [http://forums.logitech.com/t5/Mice-and-Pointing-Devices/M570-Horizontal-Scrolling/td-p/656358][1] - Logitech forum post that first alerted me to "Universal Scroll".
2. [http://forums.logitech.com/t5/Mice-and-Pointing-Devices/universal-scroll-on-marble-mouse/td-p/161730][2] - Logitech forum post linking to several Marble Mouse specific Windows friendly utilities.
3. [https://wiki.archlinux.org/index.php/Logitech_Marble_Mouse][3] - ArchLinux guide for the Logitech Marble Mouse.
4. [https://help.ubuntu.com/community/Logitech_Marblemouse_USB][4] - Reference from ArchLinux guide which had the very helpful 'Troubleshooting' section.
5. [http://forums.linuxmint.com/viewtopic.php?f=49&t=87176][5] - Honorable mention which listed basically the same information as the other guides but provides some moral support throughout the process.

[1]: http://forums.logitech.com/t5/Mice-and-Pointing-Devices/M570-Horizontal-Scrolling/td-p/656358
[2]: http://forums.logitech.com/t5/Mice-and-Pointing-Devices/universal-scroll-on-marble-mouse/td-p/161730 
[3]: https://wiki.archlinux.org/index.php/Logitech_Marble_Mouse
[4]: https://help.ubuntu.com/community/Logitech_Marblemouse_USB
[5]: http://forums.linuxmint.com/viewtopic.php?f=49&t=87176
