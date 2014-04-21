# 
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=autoconf
PKG_VERSION:=2.68
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@GNU/autoconf
PKG_MD5SUM:=864d785215aa60d627c91fcb21b05b07
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/autoconf
  SECTION:=rpi-devel
  CATEGORY:=Raspberry Pi
  SUBMENU:=Development
  TITLE:=autoconf
  DEPENDS:=+gcc +perl +m4 +perlbase-essential +perlbase-file +perlbase-class \
	+perlbase-io +perlbase-symbol +perlbase-selectsaver +perlbase-xsloader \
	+perlbase-fcntl +perlbase-config +perlbase-posix +perlbase-autoloader \
	+perlbase-tie +perlbase-cwd +perlbase-getopt +perlbase-errno \
	+perlbase-dynaloader +perlbase-data +perlbase-bytes +perlbase-text \
	@TARGET_brcm2708
  URL:=http://www.gnu.org/software/autoconf
endef

define Package/autoconf/description
	Autoconf is an extensible package of M4 macros that produce shell scripts to automatically configure software source code packages. 
	These scripts can adapt the packages to many kinds of UNIX-like systems without manual user intervention. 
	Autoconf creates a configuration script for a package from a template file that lists the operating system features that the package can use, 
	in the form of M4 macro calls.
endef

CONFIGURE_ARGS+= --datarootdir=$(STAGING_DIR)/usr/share

define Build/Compile
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR)
	SHELL="$(BASH)" $(MAKE) -C $(PKG_BUILD_DIR) install DESTDIR="$(PKG_INSTALL_DIR)"
	$(SED) 's,$(STAGING_DIR_HOST)/bin/m4,/usr/bin/m4,g' $(PKG_BUILD_DIR)/bin/autom4te
	$(SED) 's,$(STAGING_DIR_HOST)/bin/m4,/usr/bin/m4,g' $(PKG_BUILD_DIR)/bin/autoupdate
endef

define Package/autoconf/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/share/autoconf
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/** $(1)/usr/bin
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/autoconf/** $(1)/usr/share/autoconf
endef

$(eval $(call BuildPackage,autoconf))
