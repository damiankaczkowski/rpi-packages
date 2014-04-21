#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libinput
PKG_VERSION:=20140223
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://anongit.freedesktop.org/wayland/libinput
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=128f98c43b4b4da0b78b759ae8722249fc06e886
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MD5SUM:=870903db1112151fd2f94939d0fa25f2
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libinput
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=libinput
  DEPENDS:=+libmtdev +librt +udev
  URL:=http://cgit.freedesktop.org/wayland/libinput
endef

define Package/libinput/description
  This library does processing on input device events while providing an API
  to the user used for delegating more useful input events.
  Input event processing includes scaling touch coordinates, generating
  pointer events from touchpads, pointer acceleration, etc.
  It is based on the input code from the weston Wayland reference compositor.
endef

CONFIGURE_ARGS+= --disable-documentation --disable-tests

define Package/libinput/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libinput.so** $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libinput.h $(1)/usr/include/
endef

$(eval $(call BuildPackage,libinput))
