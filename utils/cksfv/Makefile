include $(TOPDIR)/rules.mk

PKG_NAME:=cksfv
PKG_VERSION:=1.3.15
PKG_RELEASE:=2
PKG_SOURCE:=$(PKG_NAME)-$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://gitlab.com/heikkiorsila/cksfv/-/archive/$(PKG_NAME)-$(PKG_VERSION)/
PKG_HASH:=b6b2390cb1cb0d62e2365683b6fa39096005efbdb6856f6763ce96be80043cc9
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Marius Dinu <marius+git@psihoexpert.ro>
PKG_LICENSE:=GPL-2.0+
PKG_LICENSE_FILES:=COPYING
include $(INCLUDE_DIR)/package.mk

define Package/cksfv
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=CRC32 checksum verification tool
  URL:=http://zakalwe.fi/~shd/foss/cksfv/
endef

define Package/cksfv/description
  cksfv is a tool for verifying CRC32 checksums in .sfv files.
endef

define Package/cksfv/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/cksfv $(1)/usr/sbin/cksfv
endef

$(eval $(call BuildPackage,cksfv))
