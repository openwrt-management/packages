#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libnetconf
PKG_VERSION:=0.8.1
PKG_RELEASE:=3
PKG_REV:=f9d1be5f224d
PKG_INSTALL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://code.google.com/p/libnetconf/
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)
PKG_LICENSE:=BSD-3-Clause

PKG_BUILD_DIR=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/libnetconf
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=NETCONF library
  URL:=$(PKG_SOURCE_URL)
  DEPENDS:= \
	+libxml2 \
	+zlib \
	+LIBNETCONF_ENABLE_VALIDATION:libxslt \
	+LIBNETCONF_ENABLE_URL:libcurl \
	+LIBNETCONF_USE_LIBSSH2:libssh2
  MAINTAINER:=Jiri Slachta <slachta@cesnet.cz>
endef

define Package/libnetconf/description
 libnetconf is the NETCONF library in C intended for building NETCONF clients and servers.

 libnetconf provides basic functions to connect NETCONF client and server to each other via
 SSH, to send and receive NETCONF messages and to store and work with the configuration data 
 in a datastore.
endef

define Package/libnetconf/config
menu "Configuration"
	depends on PACKAGE_libnetconf
	config LIBNETCONF_ENABLE_NOTIFICATIONS
		bool "Enable support of NETCONF Notifications (RFC 5277)"
		default y

	config LIBNETCONF_ENABLE_URL
		bool "Enable support of NETCONF URL Capability (RFC 6241)"
		default y

	config LIBNETCONF_ENABLE_VALIDATION
		bool "Enable support for configuration data validation (RFC 6241)"
		default y

	config LIBNETCONF_USE_LIBSSH2
		bool "Use libssh2 library for SSH transport."
		default y
		help
			If this config is selected, the libssh2 library is used
			for SSH transport. Otherwise the system SSH client is used.

	config LIBNETCONF_WORKING_DIR
		string "NETCONF working directory"
		default '/usr/share/libnetconf/'
		help
			Path to a working directory of libnetconf, where all
			the system files are stored, the default value is
			/usr/share/libnetconf/
endmenu
endef

CONFIGURE_ARGS+= \
	--prefix=/usr/ \
	--exec-prefix=/usr/ \
	$(if $(CONFIG_LIBNETCONF_USE_LIBSSH2)--enable,--disable)-libssh2 \
	$(if $(CONFIG_LIBNETCONF_ENABLE_NOTIFICATIONS),--enable,--disable)-notifications \
	$(if $(CONFIG_LIBNETCONF_ENABLE_URL),--enable,--disable)-url \
	$(if $(CONFIG_LIBNETCONF_ENABLE_VALIDATION),--enable,--disable)-validation \
	--with-ncworkingdirpath=$(CONFIG_LIBNETCONF_WORKING_DIR)

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libnetconf{,_ssh,_xml}*.h $(1)/usr/include/

	$(INSTALL_DIR) $(1)/usr/include/libnetconf
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libnetconf/*.h $(1)/usr/include/libnetconf/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnetconf* $(1)/usr/lib/

	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libnetconf.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libnetconf/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libnetconf.so* $(1)/usr/lib/

	$(INSTALL_DIR) $(1)$(CONFIG_LIBNETCONF_WORKING_DIR)

	$(INSTALL_DIR) $(1)/usr/share/libnetconf
	$(CP) $(PKG_INSTALL_DIR)/usr/share/libnetconf/rnglib $(1)/usr/share/libnetconf/
endef

$(eval $(call BuildPackage,libnetconf))
