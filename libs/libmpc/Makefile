#
# Copyright (C) 2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=mpc
PKG_VERSION:=1.0.1

PKG_SOURCE_URL:=http://www.multiprecision.org/mpc/download/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_MD5SUM:=b32a2e1a3daa392372fbd586d1ed3679

include $(INCLUDE_DIR)/package.mk

PKG_INSTALL:=1

define Package/libmpc
    TITLE:=libmpc
    SECTION:=rpi-libraries
    CATEGORY:=Raspberry Pi
    SUBMENU:=Libraries
    URL:=http://www.multiprecision.org
    DEPENDS:=+libgmp +mpfr
endef

define Package/libmpc/description
	Library for the arithmetic of complex numbers with arbitrarily high precision
endef

CONFIGURE_ARGS += \
	--enable-static \
	--enable-shared \
	--with-mpfr=$(STAGING_DIR)/usr \
	--with-gmp=$(STAGING_DIR)/usr

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/include \
		$(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/* \
		$(1)/usr/lib/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include/* \
		$(1)/usr/include/
endef

define Package/libmpc/install
	$(INSTALL_DIR) \
		$(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/libmpc.so* \
		$(1)/usr/lib/
endef

$(eval $(call BuildPackage,libmpc))
