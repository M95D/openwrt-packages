include $(TOPDIR)/rules.mk

PKG_NAME:=hiawatha
PKG_VERSION:=11.8
PKG_RELEASE:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://hiawatha.leisink.net/files/
PKG_HASH:=1376763750fb9a88a780bac6aba8707bc2a78f8ee089c62d433e50216a5183bd

PKG_MAINTAINER:=Marius Dinu <m95d+git@psihoexpert.ro>
PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk
include $(INCLUDE_DIR)/nls.mk

define Package/hiawatha
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=web server
  URL:=http://www.hiawatha-webserver.org/
  MENU:=1
  DEPENDS:= +zlib +HIAWATHA_XSLT:libxml2 +HIAWATHA_XSLT:libxslt $(ICONV_DEPENDS)
  # +HIAWATHA_TLS:libmbedtls
  USERID:=hiawatha:hiawatha
endef

define Package/hiawatha/description
	Hiawatha is an open source lightweight webserver with a focus on security.
endef

define Package/hiawatha/config
  config HIAWATHA_TLS
    bool "TLS"
    depends on PACKAGE_hiawatha
    #select PACKAGE_libmbedtls
    default y
    help
      Support encrypted HTTP connections (TLS)
      Uses libmbedtls.
  config HIAWATHA_RPROXY
    bool "Reverse proxy"
    depends on PACKAGE_hiawatha
    default y
    help
      Support reverse-proxy functionality.
      This enables hiawatha to function as a caching and filtering proxy between internet clients and a LAN web server.
      Hiawatha will filter incomming connections, offload TLS encryption, and cache requests (if enabled).
      Local LAN web server will be protected from DDoS attacks, exploits, abuse, and it won't need to do any TLS encryption.
  config HIAWATHA_CACHE
    bool "Cache"
    depends on PACKAGE_hiawatha
    default y
    help
      Enables memory cache for files, including reverse proxy requests.
  config HIAWATHA_XSLT
    bool "XSLT"
    depends on PACKAGE_hiawatha
    select PACKAGE_libxml2
    select PACKAGE_libxslt
    default y
    help
      Enable use of XSLT (Extensible Stylesheet Language Transformations)
      as a templates for directory listing, custom error pages, etc.
  config HIAWATHA_URLTOOLKIT
    bool "URL toolkit"
    depends on PACKAGE_hiawatha
    default y
    help
      Enables rewriting requests according to regexp rules.
      This can be used for filtering, ip banning, redirects, etc.
  config HIAWATHA_MONITOR
    bool "Monitoring"
    depends on PACKAGE_hiawatha
    default n
    help
      Enables logging of statistical information to a hiawatha monitoring server.
      This tool is specific to hiawatha.
endef

# Hiawatha needs mbedtls v3.x with MBEDTLS_THREADING_PTHREAD and MBEDTLS_THREADING_C.
# mbedtls in OpenWrt is too old to use. Hiawatha install it's own mbedtls.
# When OpenWrt will update mbedtls, replace the -DUSE_SYSTEM_MBEDTLS=OFF with the one commented.
CMAKE_OPTIONS += -DLIBXSLT_INCLUDE_DIR="$(STAGING_DIR)/usr/include/libxslt" \
	-DENABLE_IPV6=$(if $(CONFIG_IPV6),ON,OFF) \
	-DENABLE_TLS=$(if $(CONFIG_HIAWATHA_TLS),ON,OFF) \
	-DENABLE_RPROXY=$(if $(CONFIG_HIAWATHA_RPROXY),ON,OFF) \
	-DENABLE_CACHE=$(if $(CONFIG_HIAWATHA_CACHE),ON,OFF) \
	-DENABLE_XSLT=$(if $(CONFIG_HIAWATHA_XSLT),ON,OFF) \
	-DENABLE_TOOLKIT=$(if $(CONFIG_HIAWATHA_URLTOOLKIT),ON,OFF) \
	-DENABLE_MONITOR=$(if $(CONFIG_HIAWATHA_MONITOR),ON,OFF) \
	-DENABLE_TOMAHAWK=OFF \
	-DUSE_SYSTEM_MBEDTLS=OFF \
#	-DUSE_SYSTEM_MBEDTLS=$(if $(CONFIG_HIAWATHA_TLS),ON,OFF) \


define Package/hiawatha/conffiles
/etc/hiawatha
endef

define Package/hiawatha/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ipkg-install/usr/sbin/hiawatha $(1)/usr/sbin/hiawatha
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ipkg-install/usr/sbin/wigwam $(1)/usr/sbin/wigwam
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ipkg-install/usr/sbin/cgi-wrapper $(1)/usr/sbin/cgi-wrapper
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/ipkg-install/usr/bin/ssi-cgi $(1)/usr/bin/ssi-cgi

# Remove this section when switching to shared mbedtls
  ifeq ($(CONFIG_HIAWATHA_TLS),y)
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/ipkg-install/usr/lib/hiawatha/libmbed* $(1)/usr/lib/
  endif

	$(INSTALL_DIR) $(1)/etc/hiawatha
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/ipkg-install/etc/hiawatha/hiawatha.conf $(1)/etc/hiawatha/hiawatha.conf
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/ipkg-install/etc/hiawatha/mimetype.conf $(1)/etc/hiawatha/mimetype.conf
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/ipkg-install/etc/hiawatha/cgi-wrapper.conf $(1)/etc/hiawatha/cgi-wrapper.conf
  ifeq ($(CONFIG_HIAWATHA_XSLT),y)
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/ipkg-install/etc/hiawatha/index.xslt $(1)/etc/hiawatha/index.xslt
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/ipkg-install/etc/hiawatha/error.xslt $(1)/etc/hiawatha/error.xslt
  endif

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/hiawatha.init $(1)/etc/init.d/hiawatha
endef

$(eval $(call BuildPackage,hiawatha))
