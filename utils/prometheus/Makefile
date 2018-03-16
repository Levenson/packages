include $(TOPDIR)/rules.mk

PKG_NAME:=prometheus
PKG_VERSION:=2.2.1
PKG_RELEASE:=2

PKG_SOURCE:=prometheus-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/prometheus/prometheus.git
PKG_SOURCE_DATE:=2018-03-13
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_MIRROR_HASH:=d3f44c7a0ea546a61d403ddd02f0b896ba63612773bf72369564932110e03c62
PKG_CAT:=zcat

include $(INCLUDE_DIR)/package.mk

define Package/prometheus
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Prometheus, is a systems and service monitoring system.
  #DESCRIPTION:=This variable is obsolete. use the Package/name/description define instead!
  URL:=https://prometheus.io
endef

define Build/Compile
	mkdir -p $(PKG_BUILD_DIR)/src/github.com/prometheus
	ln -s $(PKG_BUILD_DIR) $(PKG_BUILD_DIR)/src/github.com/prometheus/prometheus
	GOPATH=$(PKG_BUILD_DIR)	$(MAKE) GOARM=7 GOARCH=$(ARCH) -C $(PKG_BUILD_DIR) build
endef

define Package/conffiles
	/etc/config/prometheus
	/etc/prometheus/prometheus.yaml
endef

define Package/prometheus/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/prometheus $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/promtool $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/prometheus
	$(INSTALL_DIR) $(1)/etc/prometheus/console_libraries
	$(INSTALL_DIR) $(1)/etc/prometheus/consoles

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/prometheus.config $(1)/etc/config/prometheus
	$(INSTALL_CONF) ./files/prometheus.yaml $(1)/etc/prometheus/

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/prometheus.init $(1)/etc/init.d/prometheus
	$(INSTALL_DIR) $(1)/etc/collectd/conf.d
endef

$(eval $(call BuildPackage,prometheus))
