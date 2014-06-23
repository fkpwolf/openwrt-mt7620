#
# Copyright (C) 2006-2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=mt7610

PKG_REV:=2714
PKG_VERSION:=r$(PKG_REV)
PKG_RELEASE:=6

PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
PKG_KCONFIG:=MT7610_AP MT7610_AP_V24_DATA_STRUCTURE MT7610_AP_WSC MT7610_AP_WSC_V2

define KernelPackage/mt7610
  CATEGORY:=MT7610
  TITLE:=Driver for mt7610
  DEPENDS:=@TARGET_ramips +wireless-tools +maccalc
  DEFAULT:=y if (CONFIG_TARGET_ramips_mt7610n || CONFIG_TARGET_ramips_mt7610a)
  FILES:=$(PKG_BUILD_DIR)/mt7610_ap/mt7610_ap.ko
  AUTOLOAD:=$(call AutoLoad,50,mt7610_ap)
endef

define KernelPackage/mt7610/description
 This package contains a driver for MT7610
endef

define KernelPackage/mt7610/config
	source "$(SOURCE)/config.in"
endef

SOURCE_DIR:=$(PKG_BUILD_DIR)
export SOURCE_DIR

MAKEOPTS:= -C $(LINUX_DIR) \
                ARCH="$(LINUX_KARCH)" \
                CROSS_COMPILE="$(TARGET_CROSS)" \
                M="$(PKG_BUILD_DIR)/mt7610_ap" \
                CONFIG_MT7610_AP=m \
		$(foreach c, $(PKG_KCONFIG),$(if $(CONFIG_$c),CONFIG_$(c)=y)) \
		modules

define Build/Prepare
	$(call Build/Prepare/Default)
	echo "----wori"
	echo $(PKG_BUILD_DIR)
	$(CP) -r files/. $(PKG_BUILD_DIR)
endef


define Build/Compile
	$(MAKE) $(MAKEOPTS)
endef

define KernelPackage/mt7610/install
	$(INSTALL_DIR) $(1)/lib/wifi
	$(INSTALL_DATA) ./files/mt7610.sh $(1)/lib/wifi
endef

$(eval $(call KernelPackage,mt7610))
