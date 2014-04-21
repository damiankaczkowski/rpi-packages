#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=opengalax
PKG_VERSION:=0.4
PKG_RELEASE:=1
PKG_REV:=13375c67c59ee41e85c2e12bba7de8ceb858176b

PKG_SOURCE:=$(PKG_REV).tar.gz
PKG_SOURCE_URL:=https://github.com/poliva/opengalax/archive/
PKG_MD5SUM:=7fecb9ccd04abe397968b545839160b4
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_REV)
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/opengalax
  SECTION:=rpi-drivers
  CATEGORY:=Raspberry Pi
  SUBMENU:=Drivers
  TITLE:=OpenGalax - touchscreen daemon
  DEPENDS:=
  URL:=https://github.com/poliva/opengalax
endef

define Package/opengalax/description
	Opengalax is a Linux userland input device driver for touchscreen panels manufactured by
	EETI: eGalax, eMPIA, TouchKit, Touchmon, HanTouch, D-WAV Scientific Co, eTurboTouch, Faytech,
	PanJit, 3M MicroTouch EX II, ITM, etc.
	At the moment only PS/2 (via serio_raw) interface is supported.
endef

define Build/Compile
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_FLAGS)
endef

define Build/Install
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_INSTALL_FLAGS) install
endef

define Package/opengalax/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/opengalax $(1)/usr/bin
	$(INSTALL_BIN) ./files/opengalax.init $(1)/etc/init.d/opengalax
endef

$(eval $(call BuildPackage,opengalax))
