#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=weston
PKG_VERSION:=20140422
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://anongit.freedesktop.org/wayland/weston
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=4c3661fd7bd6a9970a9cc6b7f9573b8617401bd8
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MD5SUM:=f2c2353c10fcb897661edbce0f812a93
PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/weston
  SECTION:=rpi
  CATEGORY:=Raspberry Pi
  TITLE:=Weston
  DEPENDS:=+rpi-userland +libwayland +libXcursor +cairo +libxcb +libxcb-composite \
	+libxcb-shape +libxcb-xfixes +libxcb-render +libxcb-shm +libxkbcommon +udev \
	+libmtdev +libjpeg +xkeyboard-config \
	+WESTON_ENABLE_LIBUNWIND:libunwind \
	+WESTON_ENABLE_DBUS:dbus \
	+WESTON_ENABLE_WESTON_LAUNCH:libpam
  URL:=http://cgit.freedesktop.org/wayland/weston
  MENU:=1
endef

define Package/weston/description
  The Weston compositor is a minimal and fast compositor and is suitable for many embedded and mobile use cases.
endef

define Package/weston/config
	source "$(SOURCE)/Config.in"
endef

TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include/interface/vcos/pthreads -I$(STAGING_DIR)/usr/include/interface/vchiq_arm -DUSE_VCHIQ_ARM
TARGET_LDFLAGS += -rdynamic
MAKE_VARS += LIBS="-lEGL -lGLESv2 -lbcm_host -lvcos" 

CONFIGURE_ARGS+= \
	--disable-setuid-install --disable-drm-compositor --disable-x11-compositor \
	--disable-rdp-compositor --disable-headless-compositor --disable-colord \
	--disable-resize-optimization --disable-xwayland-test --enable-clients \
	--enable-demo-clients-install

ifeq ($(CONFIG_WESTON_ENABLE_CAIRO_GLESV2),y)
	CONFIGURE_ARGS+= --with-cairo=glesv2
else
	CONFIGURE_ARGS+= --with-cairo=image
endif

ifeq ($(CONFIG_WESTON_ENABLE_LIBUNWIND),y)
	CONFIGURE_ARGS+= --enable-libunwind
else
	CONFIGURE_ARGS+= --disable-libunwind
endif

ifeq ($(CONFIG_WESTON_ENABLE_DBUS),y)
	CONFIGURE_ARGS+= --enable-dbus
else
	CONFIGURE_ARGS+= --disable-dbus
endif

ifeq ($(CONFIG_WESTON_ENABLE_WESTON_LAUNCH),y)
	CONFIGURE_ARGS+= --enable-weston-launch
else
	CONFIGURE_ARGS+= --disable-weston-launch
endif

ifeq ($(CONFIG_WESTON_ENABLE_EGL),y)
	CONFIGURE_ARGS+= --enable-egl --enable-simple-egl-clients
else
	CONFIGURE_ARGS+= --disable-egl --disable-simple-egl-clients
endif

ifeq ($(CONFIG_WESTON_ENABLE_WAYLAND_COMPOSITOR),y)
	CONFIGURE_ARGS+= --enable-wayland-compositor
else
	CONFIGURE_ARGS+= --disable-wayland-compositor
endif

ifeq ($(CONFIG_WESTON_ENABLE_FBDEV_COMPOSITOR),y)
	CONFIGURE_ARGS+= --enable-fbdev-compositor
else
	CONFIGURE_ARGS+= --disable-fbdev-compositor
endif

ifeq ($(CONFIG_WESTON_ENABLE_XWAYLAND),y)
	CONFIGURE_ARGS+= --enable-xwayland
else
	CONFIGURE_ARGS+= --disable-xwayland
endif

CONFIGURE_ARGS+= --enable-rpi-compositor WESTON_NATIVE_BACKEND="rpi-backend.so"

define Package/weston/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/lib/weston \
		$(1)/usr/share/weston

	$(CP) $(PKG_INSTALL_DIR)/usr/bin/** $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-desktop-shell $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-keyboard $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-screenshooter $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-simple-im $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston/*.so $(1)/usr/lib/weston/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/weston/*.* $(1)/usr/share/weston/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/lib/weston \
		$(1)/usr/share/weston \
		$(1)/usr/include/weston
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/** $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/weston/*.h $(1)/usr/include/weston/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-desktop-shell $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-keyboard $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-screenshooter $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston-simple-im $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/weston/*.so $(1)/usr/lib/weston/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/weston/*.* $(1)/usr/share/weston/
endef

define Package/weston/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	echo "Add this to /etc/profile:"
	echo "export XDG_RUNTIME_DIR=/tmp/xdg"
	echo
	echo "And this, to /etc/init.d/boot, after line mkdir -p /tmp/.uci:"
	echo "mkdir -p /tmp/xdg"
	echo "chmod 0700 /tmp/xdg"
fi
exit 0
endef

$(eval $(call BuildPackage,weston))
