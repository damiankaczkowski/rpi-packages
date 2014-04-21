#
# Copyright (C) 2007-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libX11
PKG_RELEASE:=1
PKG_VERSION:=1.6.2

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/lib/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=c35d6ad95b06635a524579e88622fdb5
PKG_FIXUP:=libtool
PKG_INSTALL:=1
PKG_BUILD_DEPENDS:=\
	xproto \
	xextproto \
	xtrans \
	kbproto \
	inputproto

include $(INCLUDE_DIR)/package.mk

define Package/libX11
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  DEPENDS:=+libXdmcp +libXau +libxcb
  TITLE:=libX11
  URL:=http://xorg.freedesktop.org/
endef

define Feature/libX11
  TARGET_NAME:=drawing-backend
  TARGET_TITLE:=Drawing Backend
  NAME:=libX11
endef

define Package/libX11/description
	This package provides a client interface to the X Window System, otherwise known as 'Xlib'.
	It provides a complete API for the basic functions of the window system.
endef

define Build/Configure
	$(call Build/Configure/Default, \
		--enable-malloc0returnsnull \
		--enable-xf86bigfont \
		--without-xcb \
		--without-launchd \
	)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/src/util \
		CC="$(HOSTCC)" \
        CFLAGS="$(HOST_CFLAGS)" \
        CPPFLAGS="$(HOST_CPPFLAGS)" \
        LDFLAGS="$(HOST_LDFLAGS)"

	$(call Build/Compile/Default)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib/pkgconfig,include/X11}
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.{so*,la,a} \
		$(1)/usr/lib/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* \
		$(1)/usr/lib/pkgconfig/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/X11/* \
		$(1)/usr/include/X11/
endef

define Package/libX11/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/share/X11
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/*.so* \
		$(1)/usr/lib/
	$(CP)  \
		$(PKG_INSTALL_DIR)/usr/share/X11/* \
		$(1)/usr/share/X11
endef

$(eval $(call Feature,libX11))
$(eval $(call BuildPackage,libX11))
