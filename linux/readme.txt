Tested 2018 on Ubuntu 18.04

**Developer friendly German + Russian keyboard layout, based on standard
 American layout**

Tested on 2016-06-13 - works with Ubuntu 14.04 and 16.04.

November 2009-2016 - RUSSIAN
----------------------------
For russian keyboard extend the official ubuntu layout file

    sudo sh -c 'cat ~/projects/keyboard-layouts/linux/replacement_ru >> /usr/share/X11/xkb/symbols/ru'

Then remove the old 'Legacy' definition block

    sudo vi /usr/share/X11/xkb/symbols/ru

Remove the old block

    xkb_symbols "legacy" {
      ...
    }

Since Ubuntu 14.04 run `sudo dpkg-reconfigure xkb-data` to reconfigure.

July 2009-2016 - DEVELOPER
--------------------------
Very simple solution without much customization:

* just use the American keyboard layout - it is pretty good for coding. For Ruby too.
* use compose key to create all the possible accented characters
* modify the LSGT key (the key to the right of the left shift key on an
  European keyboard) to produce a dead umlaut and sharp S

    sudo cp /usr/share/X11/xkb/symbols/us /usr/share/X11/xkb/symbols/us.bak
    sudo vi /usr/share/X11/xkb/symbols/us

at the end of the following block (before the closing brace)

    xkb_symbols "basic" {
    }

append this

    key <LSGT> { [ dead_diaeresis, ssharp         ] };

Since Ubuntu 14.04 run `sudo dpkg-reconfigure xkb-data` to reconfigure.




OLD, OBSOLETE (pre 2010 configuration)
======================================

Obsolete, for reference only.

March 2009
----------
install with `sh install.sh`

Diagnostics
-----------
Helpful commands:

    xprop -root | grep XKB
    gconftool-2 -R /desktop/gnome/peripherals/keyboard/kbd

For me it returns (january 2009)

    layouts = [de	hacker,ru	phonetic_de]
    options = [altwin	altwin:hyper_win,grp	grp:shift_caps_toggle,grp	grp:lwin_toggle]

A layout can be activated:
* by logging out an in to the gnome session
* `setxkbmap -layout "de(hacker)" -option -option grp:shift_caps_toggle -option caps:vimhacker`
* `killall gnome-settings-daemon; sleep 2; gnome-settings-daemon`


XKB Setup
---------

But before you start it is wise to backup your whole `/usr/share/X11/xkb` folder.

    sudo cp -r /usr/share/X11/xkb /usr/share/X11/xkb.bak

Check the `InputDevice` section of your /etc/X11/xorg.conf, related to the
keyboard. Look for the Option `"XkbRules"`. For the newer X.org based systems
it should be `"xorg"`. This is the file name you have to look for in
`/usr/share/X11/xkb/rules/`.

After copying/linking new keyboard layouts to `/usr/share/X11/xkb/symbols/` you will
need to adjust `xorg.lst` and `xorg.xml` or evdev.xml on newer Ubuntu (Intrepid)
to make new layouts available for
example in gnome's System -> Preferences -> Keyboard Preferences -> Layouts.

In addition under Keyboard Preferences:

* Group Shift/Lock behavior:
    + Shift+CapsLock changes group
    + (probably) Right Win-key switches group **while pressed**
* Alt/Win key behavior
    + Alt is mapped to the right Win-key and Super to Menu

For my notebook
---------------
My Thinkpad T43 does not have any Windows-logo key.

So the idea is to:
* use the CapsLock as a Super_L key and mod4 modifier
* for switching to caps (not needed very often) use less desirable Right-Win-Logo on the external keyboard and "Access IBM" Button on the notebook

You can easily achieve this effect with `xmodmap`. Example .Xmodmap file:
  s. subversion
Note: Xmodmap is now obsolete

Resources
---------
[1] An Unreliable Guide to XKB Configuration http://www.charvolant.org/~doug/xkb/html/xkb.html
[2] http://people.uleth.ca/~daniel.odonnell/Blog/custom-keyboard-in-linuxx11
