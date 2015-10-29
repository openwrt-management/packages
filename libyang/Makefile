#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libyang
PKG_VERSION:=0.3.4
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_LICENSE:=GPL-2.0+
PKG_MAINTAINER:=Luka Perkov <luka@openwrt.org>

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/CESNET/libyang.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=952489bbc94c83e23afdd4a396114996efdc5c9a
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libyang
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=YANG data modeling language library
  URL:=https://github.com/CESNET/libyang
  DEPENDS:=+libpcre
endef

define Package/yanglint
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=YANG data modeling language utility
  URL:=https://github.com/CESNET/libyang
  DEPENDS:=+libyang
endef

CMAKE_OPTIONS = \
	-DCMAKE_INSTALL_PREFIX=/

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/lib/libyang.so* \
		$(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/include/libyang
	$(CP) \
		$(PKG_INSTALL_DIR)/include/libyang/* \
		$(1)/usr/include/libyang/
endef

define Package/libyang/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) \
		$(PKG_INSTALL_DIR)/lib/libyang.so* \
		$(1)/usr/lib/
endef

define Package/yanglint/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) \
		$(PKG_BUILD_DIR)/yanglint \
		$(1)/usr/bin/
endef

$(eval $(call BuildPackage,libyang))
$(eval $(call BuildPackage,yanglint))
