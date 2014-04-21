#
# Copyright (C) 2008-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libunwind
PKG_VERSION:=1.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://download.savannah.gnu.org/releases/libunwind
PKG_MD5SUM:=fb4ea2f6fbbe45bf032cd36e586883ce

include $(INCLUDE_DIR)/package.mk

define Package/libunwind
  SECTION:=rpi-libraries
  CATEGORY:=Raspberry Pi
  SUBMENU:=Libraries
  TITLE:=The libunwind project
  DEPENDS:=+liblzma
  URL:=http://www.nongnu.org/libunwind/
endef

define Package/libunwind/description
	The primary goal of this project is to define a portable and efficient C programming interface (API) to determine the call-chain of a program.
	The API additionally provides the means to manipulate the preserved (callee-saved) state of each call-frame and to resume execution at any point in 
	the call-chain (non-local goto). The API supports both local (same-process) and remote (across-process) operation. As such, the API is useful in a 
	number of applications.
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)
	$(MAKE) -C $(PKG_BUILD_DIR) DESTDIR=$(PKG_INSTALL_DIR) install
endef

define Package/libunwind/install	
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libunwin*.so* $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include \
		$(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/include/*.h $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libunwin*.so* $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/* $(1)/usr/lib/pkgconfig/
endef

$(eval $(call BuildPackage,libunwind))
