#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libwayland
PKG_VERSION:=20141123
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://anongit.freedesktop.org/wayland/wayland
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=59f255d66e4dad89f633deec535482faa8bd8561
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MD5SUM:=b9f232b4c1f1cc395237f25b247eecc5
PKG_FIXUP:=libtool
PKG_BUILD_DEPENDS:=\
	libffi/host \
	libexpat/host \
	libwayland/host

PKG_INSTALL:=1
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk

define Package/libwayland
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=Wayland Libraries
  DEPENDS:=+libffi +libexpat +libpthread +librt
  URL:=http://cgit.freedesktop.org/wayland/wayland
endef

define Package/libwayland/description
  Wayland is intended as a simpler replacement for X, easier to develop and maintain.
  This package contains wayland libraries.
endef

CONFIGURE_ARGS+= --disable-documentation

HOST_CONFIGURE_CMD = $(BASH) ./autogen.sh

define Host/Prepare
        $(HOST_UNPACK)
endef

define Host/Configure
	( cd "$(HOST_BUILD_DIR)"; ./autogen.sh $(HOST_CONFIGURE_ARGS) --disable-documentation)
endef

define Build/Configure
	( cd "$(PKG_BUILD_DIR)"; ./autogen.sh $(TARGET_CONFIGURE_ARGS) --disable-documentation)
	$(call Build/Configure/Default)
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/*.h $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/share/aclocal
	$(CP) $(PKG_INSTALL_DIR)/usr/share/aclocal/wayland-scanner.m4 $(1)/usr/share/aclocal
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libwayland*.so* $(1)/usr/lib
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/wayland-client.pc $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/wayland-cursor.pc $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/wayland-server.pc $(1)/usr/lib/pkgconfig
	$(CP) ./files/wayland-scanner.pc $(1)/usr/lib/pkgconfig
	$(INSTALL_DIR) $(1)/usr/share/wayland
	$(CP) $(PKG_INSTALL_DIR)/usr/share/wayland/*.* $(1)/usr/share/wayland
endef

define Package/libwayland/install
	$(INSTALL_DIR) $(1)/usr/lib \
		$(1)/usr/bin \
		$(1)/usr/share/wayland
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libwayland*.so* $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/wayland-scanner $(1)/usr/bin
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libwayland))
