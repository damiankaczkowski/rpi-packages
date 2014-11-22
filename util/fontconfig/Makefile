#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=fontconfig
PKG_VERSION:=2.11.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://fontconfig.org/release/
PKG_MD5SUM:=84278204cd7f36adbea7ad8094e039ac
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/fontconfig
  SECTION:=rpi-utilities
  CATEGORY:=Raspberry Pi
  SUBMENU:=Utilities
  TITLE:=fontconfig
  DEPENDS:=+libexpat +libfreetype +libpthread
  URL:=http://fontconfig.org/
endef

define Package/fontconfig/description
	Fontconfig is a library for configuring and customizing font access.
endef

CONFIGURE_ARGS+= \
	--disable-docs \
	--with-cache-dir=/usr/share/fontconfig/cache \
	--with-arch=$(ARCH) \

CONFIGURE_VARS+= \
	ac_cv_prog_HASDOCBOOK=no \

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/freetype2
TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

define Build/Compile
	$(foreach dir,fc-case fc-lang fc-glyphname, \
		$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR)/$(dir) \
			$(MAKE_FLAGS) || exit 1;)
endef

define Build/Install
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_INSTALL_FLAGS) DOCSRC="" install
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/fontconfig $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libfontconfig.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/fontconfig.pc $(1)/usr/lib/pkgconfig/
endef

define Package/fontconfig/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libfontconfig.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/etc/fonts/conf.d
	$(CP) $(PKG_INSTALL_DIR)/etc/fonts/conf.d/*.conf $(1)/etc/fonts/conf.d/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/etc/fonts/fonts.conf $(1)/etc/fonts/
endef

$(eval $(call BuildPackage,fontconfig))
