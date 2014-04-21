# 
# Copyright (C) 2008-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=m4
PKG_VERSION:=1.4.17
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNU/$(PKG_NAME)
PKG_MD5SUM:=12a3c829301a4fd6586a57d3fcf196dc
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/m4
  SECTION:=rpi-devel
  CATEGORY:=Raspberry Pi
  SUBMENU:=Development
  TITLE:=m4
  DEPENDS:=@TARGET_brcm2708
  URL:=http://www.gnu.org/software/m4
endef

define Package/m4/description
	GNU M4 is an implementation of the traditional Unix macro processor. 
	It is mostly SVR4 compatible although it has some extensions (for example, handling more 
	than 9 positional parameters to macros). GNU M4 also has built-in functions for including files, 
	running shell commands, doing arithmetic, etc.
endef

CONFIGURE_VARS += gl_cv_func_strstr_linear=no

define Build/Compile
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR)
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR) install DESTDIR="$(PKG_INSTALL_DIR)"
endef

define Package/m4/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/m4 $(1)/usr/bin
endef

$(eval $(call BuildPackage,m4))
