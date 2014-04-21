#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pixman
PKG_RELEASE:=1
PKG_VERSION:=0.32.4

PKG_SOURCE_URL:=http://cairographics.org/releases/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_MD5SUM:=eba449138b972fbf4547a8c152fea162
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/pixman
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=pixman
  DEPENDS:=+libpthread
  URL:=http://cairographics.org/
endef

define Package/pixman/description
	Pixman is a low-level software library for pixel manipulation,
	providing features such as image compositing and trapezoid
	rasterization.
endef

CONFIGURE_ARGS+= --disable-gtk \
	--disable-arm-simd \

define Build/InstallDev
	$(INSTALL_DIR) $(1)
	$(CP) $(PKG_INSTALL_DIR)/* $(1)
endef

define Package/pixman/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/
endef

$(eval $(call BuildPackage,pixman))
