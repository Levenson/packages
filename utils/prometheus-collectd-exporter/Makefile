include $(TOPDIR)/rules.mk

PKG_NAME:=prometheus-collectd-exporter
PKG_VERSION:=0.4.0
PKG_RELEASE:=1

PKG_SOURCE:=prometheus-collectd-exporter-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/prometheus/collectd_exporter.git
PKG_SOURCE_DATE:=2018-01-22
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_MIRROR_HASH:=d3f44c7a0ea546a61d403ddd02f0b896ba63612773bf72369564932110e03c62
PKG_CAT:=zcat

include $(INCLUDE_DIR)/package.mk

define Package/prometheus-collectd-exporter
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=An exporter for collectd for consumtion by Prometheus
  DEPENDS:=+prometheus
  #DESCRIPTION:=This variable is obsolete. use the Package/name/description define instead!
  URL:=https://prometheus.io
endef

define Package/conffiles
	/etc/config/prometheus-collectd-exporter
endef


define Build/Compile
	mkdir -p $(PKG_BUILD_DIR)/src/github.com/prometheus/
	ln -s $(PKG_BUILD_DIR) $(PKG_BUILD_DIR)/src/github.com/prometheus/collectd_exporter
	GOPATH=$(PKG_BUILD_DIR)	$(MAKE) GOARM=7 GOARCH=$(ARCH) -C $(PKG_BUILD_DIR) build
endef

define Package/prometheus-collectd-exporter/install
	$(INSTALL_DIR) $(1)/usr/bin
	#?? WTF Why it is wrt
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/openwrt $(1)/usr/bin/prometheus-collectd-exporter

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/prometheus-collectd-exporter.config $(1)/etc/config/prometheus-collectd-exporter
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/prometheus-collectd-exporter.init $(1)/etc/init.d/prometheus-collectd-exporter
endef

$(eval $(call BuildPackage,prometheus-collectd-exporter))
