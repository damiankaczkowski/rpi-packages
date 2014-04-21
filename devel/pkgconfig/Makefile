#
# Copyright (C) 2007-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# blogic@openwrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=pkgconfig
PKG_VERSION:=0.28
PKG_RELEASE:=1

PKG_SOURCE_URL:=http://pkgconfig.freedesktop.org/releases/
PKG_SOURCE:=pkg-config-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/pkg-config-$(PKG_VERSION)/
PKG_MD5SUM:=aa3c86e67551adc3ac865160e34a2a0d

PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/pkgconfig
  SECTION:=rpi-devel
  CATEGORY:=Raspberry Pi
  SUBMENU:=Development
  TITLE:=pkg-config
  DEPENDS:=+glib2 @TARGET_brcm2708
  URL:=http://xorg.freedesktop.org/
endef

define Package/pkgconfig/description
	pkg-config is a helper tool used when compiling applications and libraries.
	It helps you insert the correct compiler options on the command line so an application
	can use gcc -o test test.cpkg-config --libs --cflags glib-2.0 for instance, rather
	than hard-coding values on where to find glib (or other libraries). It is language-agnostic,
	so it can be used for defining the location of documentation tools, for instance.
endef

TARGET_LDFLAGS+= -L$(STAGING_DIR)/usr/lib/libiconv-stub/lib
TARGET_CFLAGS+= -I$(STAGING_DIR)/usr/lib/libiconv-stub/include

define Package/pkgconfig/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/arm-openwrt-linux-gnu-pkg-config $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/pkg-config $(1)/usr/bin
endef

$(eval $(call BuildPackage,pkgconfig))
