#
# Copyright (C) 2006-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tslib
PKG_VERSION:=1.1
PKG_RELEASE:=1
PKG_REV:=fff6c8cb668b278b03690a7ae74a080a8066b2e5

PKG_SOURCE:=$(PKG_REV).tar.gz
PKG_SOURCE_URL:=https://github.com/kergoth/tslib/archive/
PKG_MD5SUM:=1d2d8bfd67550d1b560c29242ed402cf
PKG_BUILD_DIR:=$(BUILD_DIR)/tslib-$(PKG_REV)
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/tslib
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=tslib is an abstraction layer for touchscreen panel events
  URL:=https://github.com/kergoth/tslib
endef

define Package/tslib/description
  tslib is an abstraction layer for touchscreen panel events,
  as well as a filter stack for the manipulation of those events.
  Examples of implemented filters include jitter smoothing and the calibration transform.
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR); ./autogen.sh; echo "ac_cv_func_malloc_0_nonnull=yes" > cache-file.cache );
	$(call Build/Configure/Default, --cache-file=cache-file.cache)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include \
		$(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/** $(1)/usr/lib/pkgconfig
endef

define Package/tslib/install
	$(INSTALL_DIR) $(1)/usr/lib/ts \
		$(1)/usr/bin \
		$(1)/etc
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/ts_* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ts/*.so* $(1)/usr/lib/ts/
	$(CP) ./files/ts.conf $(1)/etc/ts.conf
	$(CP) ./files/tslib-env.sh $(1)/etc/
endef

$(eval $(call BuildPackage,tslib))
