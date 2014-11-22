rpi-packages
============

This is a package feed for OpenWRT specifically tailored for Raspberry Pi, including some rpi specific tools and extra packages that are supposed to bring OpenWRT on Raspberry Pi to the level with other available Raspberry Pi distributions.

These packages include all kinds of goodies including userland and EGL accelerated Weston..

Depencies
=========
Yes, it's not quite that simple. To get everything working, you need somethings:<br/>
 * packages feed must be installed<br/>
 * Apply patches against base system<br/>
 * Apply patches against packages tree<br />

This is also the recommended order but your mileage may vary a bit.

Incompatibilities
=================
 * xorg package feed ( disabled by default in openwrt trunk tree )<br/>

Patches?
========
Yes, most of them are just updates to "old" not yet updated versions in the openwrt trunk and some or minor enhancements and some are add-ons. All patches are available from here:<br/>
https://github.com/rpi-openwrt/patches<br/>

There you also find patching guide, whether you use patches from there or from OpenWrt's Trac.

Or you can download them from OpenWrt Trac one by one and also see the explanation of the patch,<br/>
list of patches and Trac URLs are provided in next two<br/>
chapters of this ReadMe.<br/>

How to obtain dependant package tree?
=====================================
Use following 2 commands in your trunk's root:<br/>
<pre>
./scripts/feeds update packages
rm -rf feeds/packages/devel/gcc
rm -rf feeds/packages.tmp
./scripts/feeds update -i packages
./scripts/feeds install -a -p packages
</pre>

This will install packages feed and remove gcc from it, as there can be only one gcc package.
Without GCC you lack some other packages as well, that are dependant on it, like autoconf and automake.

You can also install other feeds now, check your feeds.conf.default in trunk's root to see available feeds.
For example, to install LuCi, do the following:
<pre>
./scripts/feeds update luci
./scripts/feeds install -a -p luci
</pre>

Patches against base system
===========================
 * openwrt-populatefs-fixed.patch: https://dev.openwrt.org/raw-attachment/ticket/14770/openwrt-populatefs-fixed3.patch<br/>
 * xorg-macros-upgrade.patch: https://dev.openwrt.org/raw-attachment/ticket/14699/xorg-macros-upgrade.patch<br/>
 * udev-add-hostbuild.patch: https://dev.openwrt.org/raw-attachment/ticket/15645/udev-add-hostbuild.patch<br/>
 * lua-add-fpic.patch: https://dev.openwrt.org/raw-attachment/ticket/15647/lua-add-fpic.patch<br/>

Patches against packages tree
=============================
 * dbus.patch: https://dev.openwrt.org/raw-attachment/ticket/15646/dbus.patch<br/>

How to include this package tree in my trunk sources?
=====================================================
If you patched from rpi-openwrt/patches, you should be all set, if not - Add following line to your <b>trunk/feeds.conf.default</b>:<br />
<pre>
src-git raspberry https://github.com/rpi-openwrt/rpi-packages.git
</pre>

And then just use:<br />
<pre>
./scripts/feeds update raspberry
./scripts/feeds install -a -p raspberry
</pre>

Now you should be all set to continue with normal instructions on how to build your image with OpenWrt buildroot, but please, to save trouble and time, read the rest of this guide first.
 
Hints for compiling?
====================
To get everything working perfect, you should make sure you are using <b>gcc-linaro 4.8.x</b> as your compiler and for binutils version choose <b>linaro</b> version as well. Especially for development environment, this is very crucial step. Compiling gcc is available currently <b>only for targets with following settings</b>: uclibc and gcc-linaro-4.8

Sometimes, there are issues when trying to compile libefl and/or Elementary. Luckily, there's also a fix for that, although it needs a bit manual labor. This is because everytime, for some reason, everything isn't compiled in the right order. I got around this issue by noticing the error and then before retrying, I executed following commands:<br/>
<pre>
make package/system/udev/host/compile V=99
make package/libefl/compile V=99
make package/libefl/install V=99
</pre>

And after this, I again commanded make to build the image and all packages I wanted and everything worked out.

Recommendations
===============
I would recommend setting following to Yes.<br />
 * Kernel Modules->Input Modules->kmod-hid-generic: otherwise, you cannot use your USB keyboard(nor mice)<br/>
 * Raspberry Pi->Utilities->rpi-extendfs: allows resizing of root filesystem<br />
 * Raspberry Pi->Utilities->rpi-update: allows firmware update<br />

Installing rpi-update is important. <del>Stock OpenWrt ships with outdated firmware which will cause troubles when using graphic modes and acceleration.</del><br/>
It seems that OpenWrt has recently updated their firmware for Raspberry Pi, still, installing rpi-update and using it is not a bad idea. Regarding to one article I once read online, using old firmware might in worst case scenario even cause real damage to the rpi's hardware. It doesn't harm to update, so do it after first boot.

Also, to get Weston and acceleration working with wayland, choose rpi-userland as well.
Optionally you can test it by installing some demos too. Although, userland is marked as a dependancy when it's needed so you don't have to install it straight to image, for example, if you are planning to build a minimal image, that can be easily archieved with rpi-openwrt.. While still holding a lot of potentiality and modularity.
Provided userland ships with wayland EGL support. Thanks tomeuv.<br/>
( Source: https://github.com/tomeuv/userland/tree/wayland )

About development environment
=============================
It is highly recommended that you disable stripping of symbols, otherwise, it's very likely that you run into problems. Switch for disabling stripping is located in:<br/>
Global build settings->Binary stripping method ( set to none )
and also make sure you have unchecked other options for stripping as well.
