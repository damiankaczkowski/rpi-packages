#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# blogic@openwrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=xextproto
PKG_RELEASE:=1
PKG_VERSION:=7.2.1

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/proto/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=eaac343af094e6b608cf15cfba0f77c5

include $(INCLUDE_DIR)/package.mk

define Package/xextproto
  SECTION:=rpi-protocols
  CATEGORY:=Raspberry Pi
  SUBMENU:=Protocols
  DEPENDS:=
  TITLE:=xextproto
  URL:=http://xorg.freedesktop.org/
endef

define Package/xextproto/description
	X11 various extension wire protocol.
endef

define Build/InstallDev
	DESTDIR="$(1)" $(MAKE) -C $(PKG_BUILD_DIR)/ $(MAKE_FLAGS) install
endef

define Build/Compile
	true
endef

$(eval $(call BuildPackage,xextproto))
