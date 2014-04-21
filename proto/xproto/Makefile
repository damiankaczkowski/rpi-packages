#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# blogic@openwrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=xproto
PKG_VERSION:=7.0.25
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/proto/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=28311ef4edbbbf89f617a7f8a2e5648f

include $(INCLUDE_DIR)/package.mk

define Package/xproto
  SECTION:=rpi-protocols
  CATEGORY:=Raspberry Pi
  SUBMENU:=Protocols
  DEPENDS:=
  TITLE:=xproto
  URL:=http://xorg.freedesktop.org/
endef

define Package/xproto/description
	X11 core wire protocol and auxiliary headers.
endef

define Build/InstallDev
	DESTDIR="$(1)" $(MAKE) -C $(PKG_BUILD_DIR)/ $(MAKE_FLAGS) install
endef

define Build/Compile
	true
endef

$(eval $(call BuildPackage,xproto))
