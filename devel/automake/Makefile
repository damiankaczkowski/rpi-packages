#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=automake
PKG_VERSION:=1.11.3
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=@GNU/automake
PKG_MD5SUM:=3d72b2076eb4397ad5e9a2aace6357fd
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/automake
  SECTION:=rpi-devel
  CATEGORY:=Raspberry Pi
  SUBMENU:=Development
  TITLE:=automake
  DEPENDS:=+autoconf @TARGET_brcm2708
  URL:=http://www.gnu.org/software/automake
endef

define Package/automake/description
	Automake is a tool for automatically generating Makefile.in files compliant with the GNU Coding Standards. 
	Automake requires the use of Autoconf.
endef

CONFIGURE_ARGS+= --datarootdir=$(STAGING_DIR)/usr/share
CONFIGURE_VARS+= am_cv_prog_PERL_ithreads=no

define Build/Compile
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR)
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR) install DESTDIR="$(PKG_INSTALL_DIR)"
endef

define Package/automake/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/share \
		$(1)/usr/share/aclocal \
		$(1)/usr/share/libtool \
		$(1)/usr/share/intltool
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/aclocal $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/automake $(1)/usr/bin/
	$(CP) -r $(PKG_INSTALL_DIR)/usr/share/** $(1)/usr/share/
	ln -sf aclocal $(1)/usr/bin/aclocal-1.9
	ln -sf aclocal $(1)/usr/bin/aclocal-1.10
	ln -sf aclocal $(1)/usr/bin/aclocal-1.11
	ln -sf aclocal $(1)/usr/bin/aclocal-1.11.1
	ln -sf automake $(1)/usr/bin/automake-1.11
	$(INSTALL_DATA) ./files/m4/*.m4 $(1)/usr/share/aclocal/
	$(INSTALL_DATA) ./files/intltool/Makefile.in.in $(1)/usr/share/intltool/
	$(CP) -r ./files/libtool/** $(1)/usr/share/libtool/
	$(INSTALL_BIN) ./files/bin/libtool $(1)/usr/bin/
	$(INSTALL_BIN) ./files/bin/libtool-ucxx $(1)/usr/bin/
	$(INSTALL_BIN) ./files/bin/libtoolize $(1)/usr/bin/
endef

$(eval $(call BuildPackage,automake))
