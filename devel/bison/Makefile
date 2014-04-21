# 
# Copyright (C) 2008-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=bison
PKG_VERSION:=3.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNU/$(PKG_NAME)
PKG_MD5SUM:=a2624994561aa69f056c904c1ccb2880
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL:=1
PKG_FIXUP:=libtool

include $(INCLUDE_DIR)/package.mk

define Package/bison
  SECTION:=rpi-devel
  CATEGORY:=Raspberry Pi
  SUBMENU:=Development
  TITLE:=bison
  DEPENDS:=@TARGET_brcm2708
  URL:=http://www.gnu.org/software/bison
endef

define Package/bison/description
	Bison is a general-purpose parser generator that converts an annotated context-free grammar into a deterministic LR 
	or generalized LR (GLR) parser employing LALR(1) parser tables.
endef

define Build/Compile
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR)
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR) install DESTDIR="$(PKG_INSTALL_DIR)"
endef

define Package/bison/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/share \
		$(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liby.a $(1)/usr/lib
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/** $(1)/usr/share
endef

$(eval $(call BuildPackage,bison))
