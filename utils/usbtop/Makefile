include $(TOPDIR)/rules.mk

PKG_NAME:=usbtop
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/aguinet/usbtop
PKG_MIRROR_HASH:=634b4e4d4b9d6549b829710633453372683549ad642cc0efec68ad910a87998d
PKG_SOURCE_VERSION:=b9a26bd22b91b84bd72906c6501e61df7b13f3d6
PKG_SOURCE_DATE:=2022-04-19

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Marius Dinu <marius+git@psihoexpert.ro>

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/usbtop
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=USB bandwidth visualization tool
  URL:=https://github.com/aguinet/usbtop
  DEPENDS:= @USB +libstdcpp +libpcap
endef

define Package/usbtop/description
  usbtop is a top-like utility that shows an estimated instantaneous bandwidth on USB buses and devices.
endef

CMAKE_OPTIONS+= \
	-S $(PKG_BUILD_DIR) \
	-DBoost_NO_BOOST_CMAKE=ON \
	-DCMAKE_BUILD_TYPE=Release

TARGET_CFLAGS+= -Wno-parentheses

define Package/usbtop/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/usbtop $(1)/usr/sbin/usbtop
endef

$(eval $(call BuildPackage,usbtop))
