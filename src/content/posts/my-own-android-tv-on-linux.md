---
title: My own Android TV on linux
updated: 2026-09-08
published: 2026-09-08
description: ""
image: ""
tags:
  - self-host
  - linux
  - media
category: Guides
draft: false
---

:::warning [The Problem:] :::
I've bought a Google TV dongle some time ago, It was a cheap and cool device, and the ease of use convinced me to buy it.

At first everything was fine, I still used major streaming services and they worked ok, for the small price it was good enough.

But then.... You need to login, again, you need to update, again, you need to see 450 ads and placements for an app i don't even have installed.

Also, streaming services kept removing media i used to watch, so i decide to take the matter on my own, i've set up a small pc with jellyfin and started saving my own media.

And that, is what exposed the biggest problem of them all... 
HDR10 conversion to SDR is broken, no matter what, no matter the setting or the app.

And i'm not even starting with the closed ecosystem they are part of, which defeats the whole own my shit mantra.



:::tip[The Solution] ::::
Run a custom LineageOS build of Android TV, directly on linux.

This way i'm in control of what actually runs on it, and i'm always counting on a actual linux desktop behind, so even if the Android part is not behaving, i'm not having to get mad and change my plans.


# The Plan:

- Get a Debian ISO (13.6 in this case)
- Delete Microslop joke OS
- Install Debian, Waydroid and the LineageOS Image for android TV

I've been lucky (or unlucky) enough to be the tech-support for basically whoever knows me, so with time i've accumulated quite some tech that is technically working, but it's somewhat old and not really good for their intended use.

in this case, a small ASUS laptop with all the classic flaws, soldered RAM, soldered EMMC, and so on.

But if Windows barely run on this thing, a linux distro is comfortably running with spare RAM.


---

# The Execution:

#### 1) Installing debian is easy, just get the ISO, flash an USB drive and boot it.

::: note I've used the 4.7Gb DVD version so i don't need to connect to internet to install, but the net installer is fine too. :::

After booting the first time, you'll need to add your user to the sudoers, because linux.

Fortunately that is not hard, just open a terminal and digit:

```
su -
adduser <username> sudo
exit
```

then reboot the system to apply the changes.


---

#### 2) Then it's the turn of Waydroid:

Install the deps:

```
sudo apt install curl ca-certificates -y
```


::: warning You will need to add the backports repo to debian, because they hate you. :::

Go to the following folder:
`/etc/apt/sources.list.d`

open a terminal and enter:
```
sudo nano debian-backports.sources
```

paste the following code and save (CTRL+X , then confirm to save):
```
Types: deb deb-src
URIs: http://deb.debian.org/debian
Suites: trixie-backports
Components: main
Enabled: yes
Signed-By: /usr/share/keyrings/debian-archive-keyring.gpg
```

Now you can (hopefully) Install waydroid:
```
sudo apt update && sudo apt install waydroid -y
```


---

### 3) Get the Android TV Image

::github{repo="WayDroid-ATV/waydroid-androidtv-builds"}

I've used the LineageOS 23 (Android 16) Build (20260403)

Following the recommended method of installation, open a terminal and digit:
```
sudo waydroid init -f \
  -c https://waydroid-atv.github.io/ota/a16-tv/system \
  -v https://waydroid-atv.github.io/ota/a16-tv/vendor \
  -r lineage \
  -s GAPPS
```
Then:
```
sudo waydroid upgrade
```

Then be sure to set the BT remote skip flag:
<sub>!! You may need to run it with sudo.</sub>
```
echo atv.setup.bt_remote_pairing=false >> /var/lib/waydroid/waydroid_base.prop
```


Reboot the system to apply the changes, trying to run it directly gave me a black screen.

### 4) The BT Remote

If you had a Google TV, you had a remote with it, and you could use it to control your new Linux TV, just pair the device using the linux bluetooth adapter if you have one, and it should automatically work.

::: warning Do not try to pair the Remote using the android TV interface, it wont work. :::

If you don't have bluetooth on the linux pc, you can always use a remote app from your phone to directly control mouse and keyboard.