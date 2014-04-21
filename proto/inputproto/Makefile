#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# blogic@openwrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=inputproto
PKG_VERSION:=2.3
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/proto/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=94db391e60044e140c9854203d080654

include $(INCLUDE_DIR)/package.mk

define Package/inputproto
  SECTION:=rpi-protocols
  CATEGORY:=Raspberry Pi
  SUBMENU:=Protocols
  DEPENDS:=
  TITLE:=inputproto
  URL:=http://xorg.freedesktop.org/
endef

define Package/inputproto/descriptiong
	Input extension headers1.
endef

define Build/InstallDev
	DESTDIR="$(1)" $(MAKE) -C $(PKG_BUILD_DIR)/ $(MAKE_FLAGS) install
endef

define Build/Compile
	true
endef

$(eval $(call BuildPackage,inputproto))
