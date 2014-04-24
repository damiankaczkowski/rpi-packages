#
# Copyright (C) 2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=uinput
PKG_VERSION:=$(LINUX_VERSION)
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/uinput
  SUBMENU:=Input modules
  TITLE:=User level driver support
  VERSION:=$(LINUX_VERSION)+$(PKG_VERSION)-$(BOARD)-$(PKG_RELEASE)
  KCONFIG:=CONFIG_INPUT_UINPUT
  FILES:=$(LINUX_DIR)/drivers/input/misc/uinput.$(LINUX_KMOD_SUFFIX)
  AUTOLOAD:=$(call AutoLoad,65,uinput)
  $(call AddDepends/input)
endef

define KernelPackage/uinput/description
  Adds support for user level drivers for input subsystem
  accessible under char device 10:223 - /dev/input/uinput.
endef

define Build/Compile
	true
endef

define KernelPackage/uinput/install
	$(INSTALL_DIR) $(1)/lib/modules/$(LINUX_VERSION)
endef

$(eval $(call KernelPackage,uinput))
