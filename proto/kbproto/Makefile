#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# blogic@openwrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=kbproto
PKG_VERSION:=1.0.6
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/proto/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=677ea8523eec6caca86121ad2dca0b71

include $(INCLUDE_DIR)/package.mk

define Package/kbproto
  SECTION:=rpi-protocols
  CATEGORY:=Raspberry Pi
  SUBMENU:=Protocols
  DEPENDS:=
  TITLE:=kbproto
  URL:=http://xorg.freedesktop.org/
endef

define Package/kbproto/description
	X11 XKB extension wire protocol.
endef

define Build/InstallDev
	DESTDIR="$(1)" $(MAKE) -C $(PKG_BUILD_DIR)/ $(MAKE_FLAGS) install
endef

define Build/Compile
	true
endef

$(eval $(call BuildPackage,kbproto))
