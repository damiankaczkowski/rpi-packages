#
# Copyright (C) 2007-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=harfbuzz
PKG_RELEASE:=1
PKG_VERSION:=0.9.25

PKG_SOURCE_URL:=http://www.freedesktop.org/software/harfbuzz/release/
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_MD5SUM:=491b631239f51dcd4f7934775306c4e7
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/autotools.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/harfbuzz
  SECTION:=rpi-utilities
  CATEGORY:=Raspberry Pi
  SUBMENU:=Utilities
  TITLE:=harfbuzz-ng
  DEPENDS:=+cairo +libfreetype +libstdcpp
  URL:=http://xorg.freedesktop.org/
endef

define Package/harfbuzz/description
  HarfBuzz is an OpenType text shaping engine.
endef

TARGET_CFLAGS+= -I$(STAGING_DIR)/usr/include/freetype2

WITH_FEATURES:= --disable-gtk-doc \
	--disable-gtk-doc-html \
	--disable-gtk-doc-pdf \
	--with-glib=no \
	--with-icu=no \
	--with-graphite2=no \
	--with-uniscribe=no \
	--with-coretext=no

define Host/Configure
	$(call autoreconf_host)
	(cd $(HOST_BUILD_DIR); \
		$(HOST_CONFIGURE_VARS) ./configure \
		$(HOST_CONFIGURE_ARGS) $(WITH_FEATURES); \
	)
endef

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR)
endef

define Build/Configure
	$(call autoreconf_target)
	(cd $(PKG_BUILD_DIR); \
		$(CONFIGURE_VARS) ./configure \
		$(CONFIGURE_ARGS) $(WITH_FEATURES) \
		--config-cache; \
	)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig $(1)/usr/include
	$(CP) -R $(PKG_INSTALL_DIR)/usr/include/** $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/** $(1)/usr/lib/pkgconfig/
endef

define Package/harfbuzz/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,harfbuzz))
