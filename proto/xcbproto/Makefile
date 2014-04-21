#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=xcb-proto
PKG_VERSION:=1.9
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://xcb.freedesktop.org/dist/
PKG_MD5SUM:=311817f844c45c0c3d76dd72e6560c10
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/xcbproto
  SECTION:=rpi-protocols
  CATEGORY:=Raspberry Pi
  SUBMENU:=Protocols
  BUILDONLY:=1
  TITLE:=xcbproto
  URL:=http://xcb.freedesktop.org/
endef

define Package/xcbproto/description
	The xcb-proto package provides the XML-XCB protocol
	descriptions that libxcb uses to generate the majority
	of its code and API.
endef

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/lib/pkgconfig \
		$(1)/host/share/xcb \
		$(1)/usr/lib/python2.7/site-packages/xcbgen

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/share/xcb/* \
		$(1)/host/share/xcb/

	$(INSTALL_DATA) \
		$(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* \
		$(1)/usr/lib/pkgconfig/

	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/python2.7/site-packages/xcbgen/*.* \
		$(1)/usr/lib/python2.7/site-packages/xcbgen/

	$(SED) 's,datadir=/usr,datadir=$(STAGING_DIR)/host,g' $(1)/usr/lib/pkgconfig/xcb-proto.pc
endef

$(eval $(call BuildPackage,xcbproto))
