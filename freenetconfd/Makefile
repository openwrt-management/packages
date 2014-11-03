#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=freenetconfd
PKG_VERSION:=2014-07-10
PKG_RELEASE=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://dev.freenetconf.org/freenetconfd
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=e2ab83cd0f7610f31d6101d645da7ea5640a132c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_LICENSE:=GPL-2.0+

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/freenetconfd
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=netconf server
  URL:=http://www.freenetconf.org/
  DEPENDS:=+libuci +libubox +libubus +libssh +libroxml
  MAINTAINER:=Luka Perkov <luka@openwrt.org>
endef

CMAKE_OPTIONS = \
	-DCMAKE_INSTALL_PREFIX=/usr \

define Package/freenetconfd/conffiles
/etc/config/freenetconfd
/etc/netconf/authorized_keys
/etc/netconf/netconf_host_ecdsa_key
/etc/netconf/netconf_host_rsa_key
/etc/netconf/netconf_host_dsa_key
endef

define Package/freenetconfd/install
	$(INSTALL_DIR) $(1)/sbin
	$(INSTALL_BIN) \
		$(PKG_INSTALL_DIR)/usr/bin/freenetconfd \
		$(1)/sbin/

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) \
		$(PKG_BUILD_DIR)/config/freenetconfd \
		$(1)/etc/config/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) \
		./files/freenetconfd.init \
		$(1)/etc/init.d/freenetconfd
endef

$(eval $(call BuildPackage,freenetconfd))
