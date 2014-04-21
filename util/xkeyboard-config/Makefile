#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=xkeyboard-config
PKG_VERSION:=2.10.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://www.x.org/releases/individual/data/xkeyboard-config/
PKG_MD5SUM:=997d75c722142be0d03ce4899d349d92
PKG_INSTALL:=1
PKG_BUILD_DEPENDS:= \
	intltool/host \
	xproto \
	libX11

include $(INCLUDE_DIR)/package.mk

define Package/xkeyboard-config
  SECTION:=rpi-utilities
  CATEGORY:=Raspberry Pi
  SUBMENU:=Utilities
  TITLE:=xkeyboard-config
  URL:=http://freedesktop.org/wiki/Software/XKeyboardConfig
endef

define Package/xkeyboard-config/description
	X Keyboard Configuration Database. The non-arch keyboard
	configuration database for X Window. The goal is to provide
	the consistent, well-structured, frequently released open
	source of X keyboard configuration data for X Window System
	implementations (free, open source and commercial).
	The project is targeted to XKB-based systems.
endef

define Build/Configure
	$(call Build/Configure/Default)
endef

define Package/xkeyboard-config/install
	$(INSTALL_DIR) $(1)/usr/share/X11/xkb
	$(CP) -v $(PKG_INSTALL_DIR)/usr/share/X11/xkb \
		$(1)/usr/share/X11/
endef

$(eval $(call BuildPackage,xkeyboard-config))
