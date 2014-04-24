#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=opengalax2
PKG_VERSION:=0.9
PKG_RELEASE:=2
PKG_REV:=fe4cb84f775e26a447602102914da9713dd8bc9d

PKG_SOURCE:=$(PKG_REV).tar.gz
PKG_SOURCE_URL:=https://github.com/oskarirauta/opengalax2/archive/
PKG_MD5SUM:=b04b81aa22917c956980a51a48554ef7
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_REV)
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/opengalax2
  SECTION:=rpi-drivers
  CATEGORY:=Raspberry Pi
  SUBMENU:=Drivers
  TITLE:=OpenGalax2 - touchscreen daemon utilizing tslib
  DEPENDS:=+tslib
  URL:=https://github.com/oskarirauta/opengalax2
endef

define Package/opengalax2/description
	Opengalax2 is a Linux userland input device driver for touchscreen panels manufactured by
	touchscreen vendors that are supported by tslib. Original OpenGalax touchscreen daemon 
	supported serial devices manufactured by EETI: eGalax, eMpia, TouchKit, Touchmon, HanTouch,
	D-WAV Scientific Co, eTurboTouch, Faytech, PanJit, 3M MicroTouch EX II, ITM, etc.
	This driver supports USB versions of same devices through tslib. Calibration is done 
	with tslib, user only needs to set desired display resolution.
endef

define Build/Compile
	$(MAKE_VARS) EXTRAVERSION="-rpi" $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_FLAGS)
endef

define Build/Install
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_INSTALL_FLAGS) install
endef

define Package/opengalax2/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/opengalax2 $(1)/usr/bin/
	$(INSTALL_BIN) ./files/opengalax2.init $(1)/etc/init.d/opengalax2
endef

$(eval $(call BuildPackage,opengalax2))
