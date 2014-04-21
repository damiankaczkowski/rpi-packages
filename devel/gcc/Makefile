# 
# Copyright (C) 2008 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
PKG_NAME:=gcc
PKG_RELEASE:=1

ifeq ($(CONFIG_GCC_VERSION),"4.6-linaro")
	PKG_REV:=4.6-2013.05
	PKG_VERSION:=4.6.4
	PKG_VERSION_MAJOR:=4.6
	PKG_MD5SUM:=26b48802ae1203cd99415026fbf56ed7
	PKG_COMP:=bz2
	PATCH_DIR:=./patches-4.6
else
	PKG_REV:=4.8-2013.12
	PKG_VERSION:=4.8.3
	PKG_VERSION_MAJOR:=4.8
	PKG_MD5SUM:=bd7a22ff4d1b6bb4824ef908e07bde66
	PKG_COMP:=xz
	PATCH_DIR:=./patches-4.8
endif

PKG_SOURCE_URL:=http://launchpad.net/gcc-linaro/$(PKG_VERSION_MAJOR)/$(PKG_REV)/+download/
PKG_SOURCE:=gcc-linaro-$(PKG_REV).tar.$(PKG_COMP)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-linaro-$(PKG_REV)
PKG_INSTALL:=1
PKG_FIXUP:=libtool

include $(INCLUDE_DIR)/package.mk

define Package/gcc
  SECTION:=rpi-devel
  CATEGORY:=Raspberry Pi
  SUBMENU:=Development
  TITLE:=gcc
  DEPENDS:=+mprf +binutils +objdump +libstdcpp +libmpc +zlib @TARGET_brcm2708 @USE_UCLIBC @(!GCC_USE_VERSION_4_8_0||BROKEN)
endef

define Package/gcc/description
	A toolchain for native compiling
endef

EXTRA_TARGET=$(if $(CONFIG_EXTRA_TARGET_ARCH),--enable-biarch --enable-targets=$(call qstrip,$(CONFIG_EXTRA_TARGET_ARCH_NAME))-linux-uclibc)

#STRIP:=$(STAGING_DIR_HOST)/bin/sstrip
#RSTRIP:= \
#	NM="$(TARGET_CROSS)nm" \
#	STRIP="$(STRIP) --strip-debug" \
#	STRIP_KMOD="$(TARGET_CROSS)strip --strip-debug" \
#	$(SCRIPT_DIR)/rstrip.sh

define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) 's,gcc_no_link=yes,gcc_no_link=no,' $(PKG_BUILD_DIR)/libstdc++-v3/configure
	$(SED) 's/\"CFLAGS=/\"CFLAGS=$(FPIC) /' $(PKG_BUILD_DIR)/libstdc++-v3/Makefile.am
	mkdir $(PKG_BUILD_DIR)/build
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR)/build; rm -f config.cache; \
		SHELL="$(BASH)" \
		libgcc_cv_fixed_point=no \
		glibcxx_cv_c99_math_tr1=no \
		$(TARGET_CONFIGURE_OPTS) \
		../configure \
			--target=$(REAL_GNU_TARGET_NAME) \
			--host=$(REAL_GNU_TARGET_NAME) \
			--build=$(GNU_HOST_NAME) \
			--program-prefix="" \
			--program-suffix="" \
			--prefix=$(CONFIGURE_PREFIX) \
			--exec-prefix=$(CONFIGURE_PREFIX) \
			--bindir=$(CONFIGURE_PREFIX)/bin \
			--libexecdir=$(CONFIGURE_PREFIX)/lib \
			--sysconfdir=/etc \
			--datadir=$(CONFIGURE_PREFIX)/share \
			--localstatedir=/var \
			--mandir=$(CONFIGURE_PREFIX)/man \
			--infodir=$(CONFIGURE_PREFIX)/info \
			$(DISABLE_NLS) \
			$(DISABLE_LARGEFILE) \
			$(DISAPLE_IPV6) \
			--with-bugurl="https://dev.openwrt.org" \
			--with-pkgversion="OpenWrt/Linaro GCC $(PKG_REV) $(REVISION)" \
			--enable-languages=c,c++ \
			--enable-shared \
			--disable-__cxa_atexit \
			--enable-target-optspace \
			--with-gnu-ld \
			--disable-libgomp \
			--disable-libmudflap \
			--disable-libssp \
			--with-gmp=$(STAGING_DIR)/usr \
			--with-mpc=$(STAGING_DIR)/usr \
			--disable-libstdcxx-pch \
			--disable-decimal-float \
			$(EXTRA_TARGET) \
			$(SOFT_FLOAT_CONFIG_OPTION) \
			$(call qstrip,$(CONFIG_EXTRA_GCC_CONFIG_OPTIONS)) \
	);
endef

define Build/Compile
	SHELL="$(BASH)" $(MAKE_VARS) DESTDIR="$(PKG_INSTALL_DIR)" \
		$(MAKE) -C $(PKG_BUILD_DIR)/build \
		$(MAKE_FLAGS) DESTDIR="$(PKG_INSTALL_DIR)" \
		all
endef

define Build/Install
	SHELL="$(BASH)" $(MAKE_VARS) DESTDIR="$(PKG_INSTALL_DIR)" \
		 $(MAKE) -C $(PKG_BUILD_DIR)/build \
		 $(MAKE_INSTALL_FLAGS) \
		install
endef

define Build/manualStripping
	$(STRIP) $(PKG_INSTALL_DIR)/usr/bin/$(REAL_GNU_TARGET_NAME)-gcc
	$(STRIP) $(PKG_INSTALL_DIR)/usr/bin/$(REAL_GNU_TARGET_NAME)-g++
	$(STRIP) $(PKG_INSTALL_DIR)/usr/bin/cpp
	$(STRIP) $(PKG_INSTALL_DIR)/usr/bin/gcov

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libasan.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libasan.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libasan.so.0.0.0 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libasan.so.0.0.0; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libatomic.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libatomic.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libatomic.so.1.0.0 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libatomic.so.1.0.0; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libgcc_s.so.1 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libgcc_s.so.1; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libiberty.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libiberty.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libitm.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libitm.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libitm.so.1.0.0 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libitm.so.1.0.0; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libstdc++.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libstdc++.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libstdc++.so.6.0.18 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libstdc++.so.6.0.18; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/libsupc++.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/libsupc++.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/cc1 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/cc1; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/cc1plus ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/cc1plus; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/collect2 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/collect2; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcc.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcc.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcc_eh.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcc_eh.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcc_pic.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcc_pic.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcov.a ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/libgcov.a; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/liblto_plugin.so.0.0.0 ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/liblto_plugin.so.0.0.0; \
	fi

	if [ -f $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/lto-wrapper ]; then \
		$(STRIP) $(PKG_INSTALL_DIR)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/lto-wrapper; \
	fi
endef

define Package/gcc/install
	$(INSTALL_DIR) $(1)/usr/bin \
		$(1)/usr/include \
		$(1)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)
	$(CP) -aR $(PKG_INSTALL_DIR)/usr/include/** $(1)/usr/include/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/{$(REAL_GNU_TARGET_NAME)-{g++,gcc},cpp,gcov} $(1)/usr/bin/
	ln -s $(REAL_GNU_TARGET_NAME)-g++ $(1)/usr/bin/c++
	ln -s $(REAL_GNU_TARGET_NAME)-g++ $(1)/usr/bin/g++
	ln -s $(REAL_GNU_TARGET_NAME)-g++ $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-c++
	ln -s $(REAL_GNU_TARGET_NAME)-gcc $(1)/usr/bin/gcc
	ln -s $(REAL_GNU_TARGET_NAME)-gcc $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-gcc-$(PKG_VERSION)
	$(CP) -a $(PKG_INSTALL_DIR)/usr/lib/*.a $(1)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/
	$(CP) -aR $(PKG_INSTALL_DIR)/usr/lib/gcc $(1)/usr/lib/
	$(CP) -aR $(TOOLCHAIN_DIR)/include $(1)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/
	$(CP) -a $(TOOLCHAIN_DIR)/lib/*.{a,o,so*} $(1)/usr/lib/gcc/$(REAL_GNU_TARGET_NAME)/$(PKG_VERSION)/

	#Symlink binutils                
	ln -s addr2line $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-addr2line
	ln -s ar $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-ar
	ln -s as $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-as
	ln -s c++filt $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-c++filt
	ln -s elfedit $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-elfedit
	ln -s gprof $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-gprof
	ln -s ld $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-ld
	ln -s ld.bfd $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-ld.bfd
	ln -s nm $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-nm
	ln -s objcopy $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-objcopy
	ln -s ranlib $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-ranlib
	ln -s readelf $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-readelf
	ln -s size $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-size
	ln -s strip $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-strip
	ln -s /bin/strings $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-strings
	ln -s objdump $(1)/usr/bin/$(REAL_GNU_TARGET_NAME)-objdump
endef

$(eval $(call BuildPackage,gcc))
