#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=sysrepo

PKG_LICENSE:=ASL-2.0
PKG_MAINTAINER:=Mislav Novakovic <mislav.novakovic@sartura.hr>

PKG_NAME:=sysrepo
PKG_VERSION:=0.3.10

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=80db90348e80a9e2960919d124afa647bf542bd0
PKG_SOURCE:=$(PKG_NAME)-$(PKG_SOURCE_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/sysrepo/sysrepo.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_SOURCE_VERSION)

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

CMAKE_INSTALL:=1

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libsysrepo
  SECTION:=util
  CATEGORY:=Utilities
  DEPENDS:=+libyang +libprotobuf-c +libev +libavl
  TITLE:=YANG-based system repository
endef

define Package/sysrepod
  SECTION:=util
  CATEGORY:=Utilities
  DEPENDS:=+libsysrepo
  TITLE:=sysrepo deamon
endef

define Package/sysrepoctl
  SECTION:=util
  CATEGORY:=Utilities
  DEPENDS:=+libsysrepo
  TITLE:=sysrepo cli
endef

define Package/sysrepocfg
  SECTION:=util
  CATEGORY:=Utilities
  DEPENDS:=+libsysrepo
  TITLE:=sysrepocfg
endef

CMAKE_OPTIONS += \
	-DCMAKE_BUILD_TYPE:STRING=release \
	-DENABLE_TESTS=OFF \
	-DBUILD_EXAMPLES=OFF \
	-DCMAKE_DISABLE_FIND_PACKAGE_SWIG=TRUE \
	-DGEN_LANGUAGE_BINDINGS:BOOL=FALSE \
	-DREPOSITORY_LOC:PATH=/etc/sysrepo \
	-DCMAKE_INSTALL_PREFIX:PATH=/usr

define Package/libsysrepo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/libsysrepo.so $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/libsysrepo-engine.so $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/etc/sysrepo/yang/internal
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/yang/* $(1)/etc/sysrepo/yang/internal

	$(INSTALL_DIR) $(1)/etc/sysrepo/data/internal
	$(INSTALL_BIN) ./files/sysrepo-module-dependencies.xml $(1)/etc/sysrepo/data/internal
endef

define Package/sysrepod/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/sysrepod $(1)/bin/

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/sysrepod.init $(1)/etc/init.d/sysrepod
endef

define Package/sysrepoctl/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/sysrepoctl $(1)/bin/
endef

define Package/sysrepocfg/install
	$(INSTALL_DIR) $(1)/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/sysrepocfg $(1)/bin/
endef

define Package/sysrepod/postinst
#!/bin/sh
mkdir /var/run
exit 0
endef

$(eval $(call BuildPackage,libsysrepo))
$(eval $(call BuildPackage,sysrepod))
$(eval $(call BuildPackage,sysrepoctl))
$(eval $(call BuildPackage,sysrepocfg))
