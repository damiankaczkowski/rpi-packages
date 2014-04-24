#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libevdev
PKG_VERSION:=20140422
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://anongit.freedesktop.org/libevdev
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=d55056c468360cbe2ecbe90fde60ba176c5429ce
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MD5SUM:=c3994d1bd899430c77fdec301e13192b
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libevdev
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=libevdev
  URL:=http://cgit.freedesktop.org/libevdev
endef

define Package/libevdev/description
  libevdev is a wrapper library for evdev devices. it moves the common tasks
  when dealing with evdev devices into a library and provides a library interface
  to the callers, thus avoiding erroneous ioctls, etc.
  The eventual goal is that libevdev wraps all ioctls available to evdev devices,
  thus making direct access unnecessary.
endef

CONFIGURE_ARGS+= --disable-documentation --disable-tests

define Package/libevdev/install
	$(INSTALL_DIR) \
		$(1)/usr/bin \
		$(1)/usr/lib
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/touchpad-edge-detector $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/libevdev.so** $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/include/libevdev-1.0/libevdev \
		$(1)/usr/lib \
		$(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libevdev-1.0/libevdev/libevdev-uinput.h $(1)/usr/include/libevdev-1.0/libevdev/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libevdev-1.0/libevdev/libevdev.h $(1)/usr/include/libevdev-1.0/libevdev/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libevdev.so** $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/** $(1)/usr/lib/pkgconfig/
endef

$(eval $(call BuildPackage,libevdev))
