#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/autotools.mk

PKG_NAME:=elementary
PKG_VERSION:=1.9.3
PKG_MAJOR_VERSION:=1.9
PKG_RELEASE=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://download.enlightenment.org/rel/libs/elementary/
PKG_MD5SUM:=cc75bb1c7c4c49de120ec33e413130ce
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/$(PKG_NAME)-$(PKG_VERSION)
HOST_BUILD_DEPENDS:=libefl/host
PKG_BUILD_DEPENDS:= \
	libefl/host \
	elementary/host
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/elementary
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=Elementary Library
  DEPENDS:=+rpi-userland +libwayland +libefl
  URL:=http://www.enlightenment.org
endef

define Package/elementary/description
  This is a VERY SIMPLE toolkit. It is not meant for writint extenside
  desktop applications (yet.). Small simple ones with simple neeeds.
  It is meant to make the programmers work almost brainless but give
  them lots of flexibility.
endef

define Package/elementary-devel
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=Native Elementary Development utilities
  DEPENDS:=+rpi-userland +libefl elementary
  URL:=http://www.enlightenment.org
endef

define Package/elementary-devel/description
  Package containing following utilities for native development with elementary:
  elementary_codegen, elm_prefs_cc, elementary_config, elementary_run, elementary_quicklaunch
endef

define Package/elementary-examples
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=Elementary examples
  DEPENDS:=+rpi-userland +libefl elementary
  URL:=http://www.enlightenment.org
endef

define Package/elementary-examples/description
  Some examples provided by Elementary
endef

WITH_FEATURES:= \
	--disable-rpath \
	--disable-static \
	--disable-nls \
	--disable-ecore-x \
	--disable-ecore-psl1ght \
	--disable-ecore-sdl \
	--disable-ecore-cocoa \
	--disable-ecore-win32 \
	--disable-ecore-wince \
	--disable-elocation \
	--disable-eweather \
	--disable-emap \
	--disable-debug \
	--disable-doc \
	--disable-elementary-test

HOST_CONFIGURE_VARS+= \
	PKG_CONFIG_PATH="$(STAGING_DIR_HOST)/lib/pkgconfig:$(HOST_BUILD_DIR)/pkgconfig"

HOST_CFLAGS+= \
	-fPIC

TARGET_CFLAGS+= \
	-std=gnu99 \
	-I$(STAGING_DIR)/usr/include/interface/vcos/pthreads \
	-I$(STAGING_DIR)/usr/include/interface/vmcs_host/linux \
	-DUSE_VCHIQ_ARM

define Host/Patch
	echo Host side patches disabled.
endef

define Host/Prepare
	$(call Host/Prepare/Default)
	mkdir -p $(HOST_BUILD_DIR)/pkgconfig
	cp ./files/lua.pc $(HOST_BUILD_DIR)/pkgconfig
endef

define Host/Configure
	$(call autoreconf_host)
	(cd $(HOST_BUILD_DIR); \
		$(HOST_CONFIGURE_VARS) ./configure \
		$(HOST_CONFIGURE_ARGS) $(WITH_FEATURES) \
		--disable-ecore-fb \
		--disable-ecore-wayland \
		--with-eet-eet=$(STAGING_DIR_HOST)/bin/eet \
		--with-edje-cc=$(STAGING_DIR_HOST)/bin/edje_cc \
		--disable-build-examples \
		--disable-install-examples; \
	)
endef

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR)
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) $(STAGING_DIR_HOST)/bin/elm_prefs_cc $(PKG_BUILD_DIR)/src/bin
endef

define Build/Configure
	$(call autoreconf_target)
	(cd $(PKG_BUILD_DIR); \
		ELEMENTARY_LIBS="-L$(STAGING_DIR)/lib -L$(STAGING_DIR)/usr/lib -leina -lpthread -leet -levas -lecore -lecore_evas -lecore_file -lecore_input -ledje -leo -lethumb_client -lemotion -lecore_imf -lecore_con -leio -leldbus -lefreet -lefreet_mime -lefreet_trash -ludev" \
		$(CONFIGURE_VARS) ./configure -C \
		$(CONFIGURE_ARGS) $(WITH_FEATURES) \
		--enable-ecore-fb \
		--enable-ecore-wayland \
		--with-eet-eet=$(STAGING_DIR_HOST)/bin/eet \
		--with-edje-cc=$(STAGING_DIR_HOST)/bin/edje_cc \
		--with-elementary-codegen=$(STAGING_DIR_HOST)/bin/elementary_codegen \
		--with-elm-prefs-cc=$(STAGING_DIR_HOST)/bin/elm_prefs_cc \
		--enable-build-examples \
		--enable-install-examples; \
	)
endef

define Build/Install
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_INSTALL_FLAGS) install
	mkdir -p $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src
	mv $(PKG_INSTALL_DIR)/usr/share/elementary/examples/*.c $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/
	mv $(PKG_INSTALL_DIR)/usr/share/elementary/examples/*.edc $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/
	mv $(PKG_INSTALL_DIR)/usr/share/elementary/examples/*.edj $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/
	mv $(PKG_INSTALL_DIR)/usr/share/elementary/examples/*.txt $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/
#	$(STRIP) $(PKG_INSTALL_DIR)/usr/share/elementary/examples/**
	$(CP) $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/*.edc $(PKG_INSTALL_DIR)/usr/share/elementary/examples/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/*.edj $(PKG_INSTALL_DIR)/usr/share/elementary/examples/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/*.txt $(PKG_INSTALL_DIR)/usr/share/elementary/examples/
#	$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libelementary.so.$(PKG_VERSION)
#	$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/edje/modules/*/*/*.so
#	$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/*/*/*.so
#	$(STRIP) $(PKG_INSTALL_DIR)/usr/bin/*
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/elementary-1 \
		$(1)/usr/lib/cmake/Elementary \
		$(1)/usr/lib/elementary \
		$(1)/usr/lib/edje \
		$(1)/usr/lib/pkgconfig \
		$(1)/usr/share/applications \
		$(1)/usr/share/icons \
		$(1)/usr/share/elementary \
		$(1)/usr/share/elementary/config \
		$(1)/usr/share/elementary/edje_externals \
		$(1)/usr/share/elementary/images \
		$(1)/usr/share/elementary/objects \
		$(1)/usr/share/elementary/themes
	$(CP) -R $(PKG_INSTALL_DIR)/usr/include/elementary-1/** $(1)/usr/include/elementary-1/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/cmake/Elementary/*.cmake $(1)/usr/lib/cmake/Elementary/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/lib/elementary/** $(1)/usr/lib/elementary/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/lib/edje/** $(1)/usr/lib/edje/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/elementary.pc $(1)/usr/lib/pkgconfig/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/applications/** $(1)/usr/share/applications/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/icons/** $(1)/usr/share/icons/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/config/** $(1)/usr/share/elementary/config/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/edje_externals/** $(1)/usr/share/elementary/edje_externals/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/images/** $(1)/usr/share/elementary/images/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/objects/** $(1)/usr/share/elementary/objects/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/themes/** $(1)/usr/share/elementary/themes/
endef

define Package/elementary/install
	$(INSTALL_DIR) $(1)/usr/lib \
		$(1)/usr/lib/elementary/modules/access_output/v-$(PKG_VERSION) \
		$(1)/usr/lib/elementary/modules/datetime_input_ctxpopup/v-$(PKG_VERSION) \
		$(1)/usr/lib/elementary/modules/prefs/v-$(PKG_VERSION) \
		$(1)/usr/lib/elementary/modules/test_entry/v-$(PKG_VERSION) \
		$(1)/usr/lib/elementary/modules/test_map/v-$(PKG_VERSION) \
		$(1)/usr/lib/edje/modules/elm/v-$(PKG_MAJOR_VERSION) \
		$(1)/usr/share/applications \
		$(1)/usr/share/icons \
		$(1)/usr/share/elementary/config \
		$(1)/usr/share/elementary/edje_externals \
		$(1)/usr/share/elementary/images \
		$(1)/usr/share/elementary/objects \
		$(1)/usr/share/elementary/themes
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/access_output/v-$(PKG_VERSION)/module.so \
		$(1)/usr/lib/elementary/modules/access_output/v-$(PKG_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/datetime_input_ctxpopup/v-$(PKG_VERSION)/module.so \
		$(1)/usr/lib/elementary/modules/datetime_input_ctxpopup/v-$(PKG_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/prefs/v-$(PKG_VERSION)/module.so \
		$(1)/usr/lib/elementary/modules/prefs/v-$(PKG_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/prefs/v-$(PKG_VERSION)/elm_prefs_swallow.edj \
		$(1)/usr/lib/elementary/modules/prefs/v-$(PKG_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/test_entry/v-$(PKG_VERSION)/module.so \
		$(1)/usr/lib/elementary/modules/test_entry/v-$(PKG_VERSION)/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/elementary/modules/test_map/v-$(PKG_VERSION)/module.so \
		$(1)/usr/lib/elementary/modules/test_map/v-$(PKG_VERSION)/

	$(CP) $(PKG_INSTALL_DIR)/usr/lib/edje/modules/elm/v-$(PKG_MAJOR_VERSION)/module.so \
		$(1)/usr/lib/edje/modules/elm/v-$(PKG_MAJOR_VERSION)/

	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/applications/** $(1)/usr/share/applications/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/icons/** $(1)/usr/share/icons/

	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/config/** $(1)/usr/share/elementary/config/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/edje_externals/** $(1)/usr/share/elementary/edje_externals/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/images/** $(1)/usr/share/elementary/images/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/objects/** $(1)/usr/share/elementary/objects/
	$(CP) -R $(PKG_INSTALL_DIR)/usr/share/elementary/themes/** $(1)/usr/share/elementary/themes/
endef

define Package/elementary-devel/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/share/elementary/examples_src
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/elementary_codegen $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/elementary_config $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/elementary_quicklaunch $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/elementary_run $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/elm_prefs_cc $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/elementary/examples_src/** $(1)/usr/share/elementary/examples_src/
endef

define Package/elementary-examples/install
	$(INSTALL_DIR) $(1)/usr/share/elementary/examples
	$(CP) $(PKG_INSTALL_DIR)/usr/share/elementary/examples/** $(1)/usr/share/elementary/examples/
endef

define Package/elementary/postinst
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
$(eval $(call BuildPackage,elementary))
$(eval $(call BuildPackage,elementary-devel))
$(eval $(call BuildPackage,elementary-examples))
