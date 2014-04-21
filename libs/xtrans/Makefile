#
# Copyright (C) 2007-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=xtrans
PKG_RELEASE:=1
PKG_VERSION:=1.3.2

PKG_SOURCE_URL:=http://xorg.freedesktop.org/releases/individual/lib
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_BUILD_DIR:=$(BUILD_DIR)/Xorg/$(PKG_NAME)-$(PKG_VERSION)/
PKG_MD5SUM:=050eb8f1d41e254f508bab72f1c10d6e
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/xtrans
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=xtrans
  URL:=http://xorg.freedesktop.org/
endef

define Package/xtrans/description
	xtrans includes a number of routines to make X implementations
	transport-independent; at time of writing, it includes support
	for UNIX sockets, IPv4, IPv6, and DECnet.
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/{lib/pkgconfig,include/X11/Xtrans}
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/pkgconfig/* \
		$(1)/usr/lib/pkgconfig/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/include/X11/Xtrans/* \
		$(1)/usr/include/X11/Xtrans/

	$(INSTALL_DIR) $(1)/usr/share/aclocal
	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/aclocal/* \
		$(1)/usr/share/aclocal/
endef

$(eval $(call BuildPackage,xtrans))
