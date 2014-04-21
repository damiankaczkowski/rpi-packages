#
# Copyright (C) 2007-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libXrender
PKG_RELEASE:=1
PKG_VERSION:=0.9.8

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/lib/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=2bd9a15fcf64d216e63b8d129e4f1f1c
PKG_FIXUP:=libtool
PKG_INSTALL:=1
PKG_BUILD_DEPENDS:= \
	renderproto

include $(INCLUDE_DIR)/package.mk

define Package/libXrender
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  DEPENDS:=+libX11
  TITLE:=libXrender
  URL:=http://xorg.freedesktop.org/
endef

define Package/libXrender/description
	The X Rendering Extension (Render) introduces digital image
	composition as the foundation of a new rendering model within
	the X Window System. Rendering geometric figures is accomplished
	by client-side tessellation into either triangles or trapezoids.
	Text is drawn by loading glyphs into the server and rendering
	sets of them. The Xrender library exposes this extension to X clients.
endef

CONFIGURE_VARS+= PKG_CONFIG_PATH="$(STAGING_DIR)/usr/lib/pkgconfig:$(PKG_BUILD_DIR)/pkgconfig"

CONFIGURE_ARGS+= --enable-malloc0returnsnull
 
define Build/Prepare
	$(call Build/Prepare/Default)
	mkdir -p $(PKG_BUILD_DIR)/pkgconfig
	$(CP) ./files/xorg-macros.pc $(PKG_BUILD_DIR)/pkgconfig
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib/pkgconfig,include/X11/extensions}
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.{so*,a,la} \
		$(1)/usr/lib/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* \
		$(1)/usr/lib/pkgconfig/
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/X11/extensions/* \
		$(1)/usr/include/X11/extensions/
endef

define Package/libXrender/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/
endef

$(eval $(call BuildPackage,libXrender))
