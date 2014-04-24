#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libefl
PKG_VERSION:=1.9.3
PKG_MAJOR_VERSION:=1.9
PKG_RELEASE=1

PKG_SOURCE:=efl-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://download.enlightenment.org/rel/libs/efl/
PKG_MD5SUM:=102acebaf453346d745b0974f736a5e6
PKG_BUILD_DIR:=$(BUILD_DIR)/efl-$(PKG_VERSION)
PKG_INSTALL:=1
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/efl-$(PKG_VERSION)
HOST_BUILD_DEPENDS:=udev/host
PKG_BUILD_DEPENDS:= \
	libfreetype/host \
	libpng/host \
	udev/host \
	dbus/host \
	lua/host \
	libbullet/host \
	harfbuzz/host \
	libefl/host

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/autotools.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/libefl
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=EFL Libraries
  DEPENDS:=+rpi-userland +libwayland +liblua +libbullet +libdbus +libfreetype \
	+harfbuzz +fontconfig +libjpeg +libpng +libstdcpp +udev +libxkbcommon +zlib
  URL:=http://www.enlightenment.org
endef

define Package/libefl/description
  Enlightenment Foundation Libraries
endef

define Package/libefl-devel
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=EFL Library native development tools
  DEPENDS:=libefl
  URL:=http://www.enlightenment.org
endef

define Package/libefl-devel/description
  Contains following tools: edje_cc edje_codegen, edge_decc, edje_external_instpector,
  edje_inspector, edje_pick, edje_player, edje_recc, edje_watch, eet, eeze_disk_ls,
  eeze_mount, eeze_scanner, eeze_umount, efreetd, eina-bench-cmp, eldbus-codegen,
  embryo_cc, evas_cserve2_client, evas_cserve2_debug, evas_cserve2_shm_debug,
  evas_cserve2_usage
endef

WITH_FEATURES:= --disable-nls \
	--disable-doc \
	--disable-rpath \
	--disable-systemd \
	--disable-sdl \
	--disable-drm \
	--disable-fribidi \
	--disable-tizen \
	--enable-eo-id \
	--enable-harfbuzz \
	--disable-pixman \
	--disable-gstreamer \
	--disable-gstreamer1 \
	--disable-tslib \
	--disable-audio \
	--disable-pulseaudio \
	--disable-gesture \
	--disable-xinput2 \
	--disable-xinput22 \
	--disable-xim \
	--disable-scim \
	--disable-ibus \
	--disable-libmount \
	--enable-physics \
	--disable-tile-rotate \
	--disable-multisense \
	--disable-xine \
	--disable-v4l2 \
	--disable-always-build-with-examples \
	--disable-image-loader-gif \
	--disable-image-loader-tiff \
	--with-crypto=none \
	--with-tests=none \
	--with-x11=none \
	--without-x \
	--with-glib=no \
	--without-mount \
	--without-umount \
	--without-eject \
	--disable-always-build-examples \
	--enable-lua-old \
	--enable-i-really-know-what-i-am-doing-and-that-this-will-probably-break-things-and-i-will-fix-them-myself-and-send-patches-aaa

HOST_CONFIGURE_VARS+= \
	PKG_CONFIG_PATH="$(STAGING_DIR_HOST)/lib/pkgconfig:$(HOST_BUILD_DIR)/pkgconfig"

HOST_CFLAGS+= -I$(HOST_BUILD_DIR)/include \
		-I$(STAGING_DIR_HOST)/include/bullet \
		-I$(STAGING_DIR_HOST)/include/harfbuzz \
		-fPIC

TARGET_CFLAGS+= -std=gnu99 \
	-I$(STAGING_DIR)/usr/include/bullet \
	-I$(STAGING_DIR)/usr/include/harfbuzz \
	-I$(STAGING_DIR)/usr/include/interface/vcos/pthreads \
	-I$(STAGING_DIR)/usr/include/interface/vmcs_host/linux \
	-DUSE_VCHIQ_ARM

define Host/Prepare
	$(call Host/Prepare/Default)
	mkdir -p $(HOST_BUILD_DIR)/pkgconfig
	mkdir -p $(HOST_BUILD_DIR)/include
	cp ./files/lua.pc $(HOST_BUILD_DIR)/pkgconfig/
	cp ./files/lnum_config.h $(HOST_BUILD_DIR)/include/
endef

define Host/Configure
	$(call autoreconf_host)
	(cd $(HOST_BUILD_DIR); \
		EPHYSICS_CFLAGS=-I$(STAGING_DIR_HOST)/include/bullet \
		$(HOST_CONFIGURE_VARS) ./configure \
		$(HOST_CONFIGURE_ARGS) $(WITH_FEATURES) \
		--disable-fb \
		--disable-fontconfig \
		--with-opengl=es; \
	)
endef

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR)
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) $(STAGING_DIR_HOST)/bin/edje_* $(PKG_BUILD_DIR)/src/bin/edje/
endef

define Build/Configure
	$(call autoreconf_target)
	(cd $(PKG_BUILD_DIR); \
		$(CONFIGURE_VARS) ./configure \
		$(CONFIGURE_ARGS) $(WITH_FEATURES) \
		--enable-fontconfig \
		--enable-wayland \
		--enable-fb \
		--enable-egl \
		--with-opengl=es \
		--with-bin-edje-cc=$(STAGING_DIR_HOST)/bin/edje_cc \
		--config-cache; \
	)
endef

define Build/Install
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_INSTALL_FLAGS) install
	chmod 755 $(PKG_INSTALL_DIR)/usr/bin/eeze_scanner
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include \
		$(1)/usr/lib \
		$(1)/usr/share
	$(CP) -R $(PKG_INSTALL_DIR)/usr/include/** $(1)/usr/include/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/lib/** $(1)/usr/lib/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/** $(1)/usr/share/
endef

define Package/libefl/install
	$(INSTALL_DIR) $(1)/usr/lib \
		$(1)/usr/lib/ecore/system/upower/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/ecore_evas/engines/extn/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/ecore_evas/engines/fb/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/ecore_evas/engines/wayland/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/ecore_imf/modules/wayland/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/edje/modules/emotion/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/edje/utils/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/eeze/modules/sensor/fake/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/eeze/modules/sensor/udev/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/efreet/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/ethumb/modules/emotion/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/ethumb_client/utils/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/evas/cserve2/bin/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/evas/modules/engines/fb/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/evas/modules/engines/wayland_egl/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/lib/evas/modules/engines/wayland_shm/v-$(PKG_MAJOR_VERSION)

	$(CP) -R $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/

	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ecore/system/upower/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/ecore/system/upower/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ecore_evas/engines/extn/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/ecore_evas/engines/extn/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ecore_evas/engines/fb/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/ecore_evas/engines/fb/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ecore_evas/engines/wayland/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/ecore_evas/engines/wayland/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ecore_imf/modules/wayland/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/ecore_evas/engines/wayland/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/edje/modules/emotion/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/edje/modules/emotion/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/edje/utils/v-$(PKG_MAJOR_VERSION)/epp \
		$(1)/usr/lib/edje/utils/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/eeze/modules/sensor/udev/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/eeze/modules/sensor/udev/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/eeze/modules/sensor/fake/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/eeze/modules/sensor/fake/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/efreet/v-$(PKG_MAJOR_VERSION)/* \
		$(1)/usr/lib/efreet/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ethumb/modules/emotion/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/ethumb/modules/emotion/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ethumb/modules/emotion/v-$(PKG_MAJOR_VERSION)/template.edj \
		$(1)/usr/lib/ethumb/modules/emotion/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ethumb_client/utils/v-$(PKG_MAJOR_VERSION)/* \
		$(1)/usr/lib/ethumb_client/utils/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/evas/cserve2/bin/v-$(PKG_MAJOR_VERSION)/* \
		$(1)/usr/lib/evas/cserve2/bin/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/evas/modules/engines/fb/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/evas/modules/engines/fb/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/evas/modules/engines/wayland_egl/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/evas/modules/engines/wayland_egl/v-$(PKG_MAJOR_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/evas/modules/engines/wayland_shm/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/evas/modules/engines/wayland_shm/v-$(PKG_MAJOR_VERSION)/

	$(INSTALL_DIR) $(1)/usr/share/dbus-1/services \
		$(1)/usr/share/ecore \
		$(1)/usr/share/ecore_imf \
		$(1)/usr/share/edje/include \
		$(1)/usr/share/eeze \
		$(1)/usr/share/efreet \
		$(1)/usr/share/embryo/include \
		$(1)/usr/share/eo/gdb \
		$(1)/usr/share/ethumb/frames \
		$(1)/usr/share/ethumb_client \
		$(1)/usr/share/evas \
		$(1)/usr/share/gdb/auto-load \
		$(1)/usr/share/mime/packages

	$(CP) $(PKG_INSTALL_DIR)/usr/share/dbus-1/services/** $(1)/usr/share/dbus-1/services/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/edje/include/** $(1)/usr/share/edje/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/embryo/include/** $(1)/usr/share/embryo/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/ethumb/frames/** $(1)/usr/share/ethumb/frames/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/mime/packages/edje.xml $(1)/usr/share/mime/packages/	
endef

define Package/libefl-devel/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/lib/cmake \
		$(1)/usr/share/eo/gdb \
		$(1)/usr/share/gdb/auto-load/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/** $(1)/usr/bin/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/lib/cmake/** $(1)/usr/lib/cmake/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/eo/gdb/eo_gdb.py $(1)/usr/share/eo/gdb/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/gdb/auto-load/usr/lib/*.py \
		$(1)/usr/share/gdb/auto-load/usr/lib/
endef

define Package/libefl/postinst
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

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libefl))
$(eval $(call BuildPackage,libefl-devel))
