#
# Copyright (C) 2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/cmake.mk

PKG_NAME:=libbullet
PKG_VERSION:=2.82
PKG_RELEASE:=1
PKG_REV:=2704

PKG_SOURCE:=bullet-$(PKG_VERSION)-r$(PKG_REV).tgz
PKG_SOURCE_URL:=https://bullet.googlecode.com/files/
PKG_MD5SUM:=70b3c8d202dee91a0854b4cbc88173e8
PKG_BUILD_DIR:=$(BUILD_DIR)/bullet-$(PKG_VERSION)-r$(PKG_REV)
HOST_BUILD_DIR:=$(BUILD_DIR_HOST)/bullet-$(PKG_VERSION)-r$(PKG_REV)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/host-build.mk

define Package/libbullet
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=libbullet
  DEPENDS:=+libstdcpp
  URL:=http://code.google.com/p/bullet
endef

define Package/libbullet/description
  Bullet 3D Realy-Time Multiphysics Library provides state of the art collision
  detection, soft body and rigid body dynamics.
endef

CONFIGURE_CMD:=./autogen.sh
CONFIGURE_ARGS+= --disable-demos

define Host/Configure
	(cd $(HOST_BUILD_DIR); \
		LDFLAGS="$(HOST_LDFLAGS) $(EXTRA_LDFLAGS)" \
		cmake . \
			-DBUILD_SHARED_LIBS=1 \
			-DBUILD_DEMOS=0 \
			-DBUILD_EXTRAS=1 \
			-DINSTALL_LIBS=1 \
			-DINSTALL_EXTRA_LIBS=1 \
			-DCMAKE_SYSTEM_NAME="Linux" \
			-DCMAKE_C_COMPILER=$(HOSTCC) \
			-DCMAKE_CXX_COMPILER=$(HOSTCXX) \
			-DCMAKE_SHARED_LINKER_FLAGS:STRING="$(HOST_LDFLAGS)" \
			-DCMAKE_FIND_ROOT_PATH="$(STAGING_DIR_HOST);$(TOOLCHAIN_DIR)" \
			-DCMAKE_FIND_ROOT_PATH_MODE_PROGRAM=BOTH \
			-DCMAKE_FIND_ROOT_PATH_MODE_LIBRARY=BOTH \
			-DCMAKE_FIND_ROOT_PATH_MODE_INCLUDE=BOTH \
			-DCMAKE_STRIP=: \
			-DCMAKE_BUILD_TYPE=Release; \
	)
endef

define Host/Compile
	$(HOST_MAKE_VARS) $(MAKE) -C $(HOST_BUILD_DIR) $(HOST_MAKE_FLAGS)
	$(MAKE) -C $(HOST_BUILD_DIR) DESTDIR=$(HOST_BUILD_DIR)/host-install install
endef

define Host/Install
	$(SED) 's,/usr/local,,' $(HOST_BUILD_DIR)/host-install/usr/local/lib/pkgconfig/bullet.pc
	mkdir -p $(STAGING_DIR_HOST)/include/bullet
	$(CP) -R $(HOST_BUILD_DIR)/host-install/usr/local/include/bullet/** $(STAGING_DIR_HOST)/include/bullet
	$(CP) -R $(HOST_BUILD_DIR)/host-install/usr/local/lib/** $(STAGING_DIR_HOST)/lib
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR); \
		LDFLAGS="$(TARGET_LDFLAGS) $(EXTRA_LDFLAGS)" \
		cmake . \
			-DCMAKE_INSTALL_PREFIX=/usr \
			-DBUILD_SHARED_LIBS=1 \
			-DBUILD_DEMOS=0 \
			-DBUILD_EXTRAS=1 \
			-DINSTALL_LIBS=1 \
			-DINSTALL_EXTRA_LIBS=1 \
			-DCMAKE_SYSTEM_NAME="Linux" \
			-DCMAKE_C_COMPILER=$(TARGET_CC) \
			-DCMAKE_CXX_COMPILER=$(TARGET_CXX) \
			-DCMAKE_SYSTEM_PROCESSOR=arm \
			-DCMAKE_C_COMPILER_ARG1="$(CMAKE_C_COMPILER_ARG1)" \
			-DCMAKE_CXX_COMPILER_ARG1="$(CMAKE_CXX_COMPILER_ARG1)" \
			-DCMAKE_SHARED_LINKER_FLAGS:STRING="$(TARGET_LDFLAGS)" \
			-DCMAKE_FIND_ROOT_PATH="$(STAGING_DIR);$(TOOLCHAIN_DIR)" \
			-DCMAKE_FIND_ROOT_PATH_MODE_PROGRAM=BOTH \
			-DCMAKE_FIND_ROOT_PATH_MODE_LIBRARY=ONLY \
			-DCMAKE_FIND_ROOT_PATH_MODE_INCLUDE=ONLY \
			-DCMAKE_STRIP=: \
			-DCMAKE_BUILD_TYPE=Release; \
	)
endef

define Build/Compile
	$(MAKE_VARS) $(MAKE) -C $(PKG_BUILD_DIR) $(MAKE_FLAGS)
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(PKG_INSTALL_DIR) install
endef

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/lib/pkgconfig \
		$(1)/usr/include/bullet
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/bullet.pc $(1)/usr/lib/pkgconfig
	$(CP) -R $(PKG_INSTALL_DIR)/usr/include/bullet/** $(1)/usr/include/bullet
endef

define Package/libbullet/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib
endef

$(eval $(call HostBuild))
$(eval $(call BuildPackage,libbullet))
