#
# Copyright (C) 2008-2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cairo
PKG_VERSION:=1.12.16
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://cairographics.org/releases/
PKG_MD5SUM:=a1304edcdc99282f478b995ee5f8f854
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/cairo
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=Multi-platform 2D graphics library
  DEPENDS:=+fontconfig +libpng +libexpat +libpthread +librt +libxcb-render +libxcb-shm +libxcb +pixman +libjpeg +glib2 +rpi-userland @TARGET_brcm2708
  URL:=http://cairographics.org/
endef

define Package/cairo/description
Cairo is a 2D graphics library. The cairo API provides operations similar
to the drawing operators of PostScript and PDF. Operations in cairo
include stroking and filling cubic Bézier splines, transforming and
compositing translucent images, and antialiased text rendering. All
drawing operations can be transformed by any affine transformation
(scale, rotation, shear, etc.).
endef

TARGET_CFLAGS+= $(FPIC) -D_REENTRANT -D_THREAD_SAFE -D_THREADSAFE -DUSE_VCHIQ_ARM

CONFIGURE_VARS+= \
		FONTCONFIG_LIBS="-lfreetype -lfontconfig -lz -lexpat" \

CONFIGURE_ARGS+= \
	--enable-freetype=yes \
	--disable-win32 \
	--enable-xlib-xrender=no \
	--disable-directfb --disable-xlib --without-x \
	--enable-xlib-xcb=auto --enable-win32=no --enable-win32-font=no \
	--enable-gl=no --enable-egl=yes --enable-glesv2=yes --enable-directfb=auto \
	--disable-gtk-doc --disable-gtk-doc-html --disable-gtk-doc-pdf

ifeq ($(CONFIG_GCC_VERSION),"4.6-linaro")
	CONFIGURE_ARGS+= --enable-pthread=auto
	TARGET_CFLAGS+=-DCAIRO_NO_MUTEX
else
	CONFIGURE_ARGS+= --enable-pthread=yes
endif

CONFIGURE_CMD = ./autogen.sh

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.{so*,a,la} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* $(1)/usr/lib/pkgconfig/
	$(INSTALL_DIR) $(1)/usr/include/cairo/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/include/cairo/* $(1)/usr/include/cairo/
endef

define Package/cairo/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,cairo))
