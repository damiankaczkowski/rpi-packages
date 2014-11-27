#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libinput
PKG_VERSION:=20141123
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://anongit.freedesktop.org/wayland/libinput
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=bded6ac1d1f4aa93a5ecd8d681730734392aae8c
#PKG_SOURCE_VERSION:=9c61146e9e7d9f72fa7094e32224e937d963b908
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
#PKG_MD5SUM:=de95fbde67b05872f8fbc1802c6c7357
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libinput
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=libinput
  DEPENDS:=+libevdev +libmtdev +librt +udev
  URL:=http://cgit.freedesktop.org/wayland/libinput
endef

define Package/libinput/description
  This library does processing on input device events while providing an API
  to the user used for delegating more useful input events.
  Input event processing includes scaling touch coordinates, generating
  pointer events from touchpads, pointer acceleration, etc.
  It is based on the input code from the weston Wayland reference compositor.
endef

TARGET_CFLAGS += -std=gnu99

CONFIGURE_ARGS+= --disable-documentation --disable-tests

define Package/libinput/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libinput.so** $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/include \
		$(1)/usr/lib \
		$(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libinput.h $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libinput.so** $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/** $(1)/usr/lib/pkgconfig/
endef

$(eval $(call BuildPackage,libinput))
