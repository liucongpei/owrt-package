#
# Copyright (C) 2006-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=keepalived-devel
PKG_VERSION:=1.2.13
PKG_RELEASE:=10

PKG_SOURCE:=keepalived-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:= http://www.keepalived.org/software
PKG_MD5SUM:=5f5c038139c55b5af54deefbcce64e33

PKG_BUILD_DIR:=$(BUILD_DIR)/keepalived-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/kernel.mk

define Package/keepalived-devel
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Failover and monitoring daemon for LVS clusters
  URL:=http://www.keepalived.org/
  DEPENDS:=+libopenssl +libpcre +libltdl +libpopt +ipvsadm +kmod-ipvs-core +kmod-ipvs-dh +kmod-ipvs-ftp +kmod-ipvs-lblc +kmod-ipvs-lblcr \
	+kmod-ipvs-lc +kmod-ipvs-nq +kmod-ipvs-pe-sip +kmod-ipvs-rr +kmod-ipvs-sed +kmod-ipvs-sh +kmod-ipvs-wlc +kmod-ipvs-wrr +kmod-ipt-nathelper-extra \
	+iptables-mod-nat-extra \
	+libnl-tiny

endef

define Package/keepalived-devel/description
 Failover and monitoring daemon for Linux Virtual Server (LVS) clusters.
endef

define Package/keepalived-devel/conffiles
/etc/keepalived/keepalived.conf
endef

acvar=$(subst -,_,$(subst .,_,$(subst /,_,$(1))))

CONFIGURE_ARGS+= \
	--enable-shared \
	--disable-static \
	--disable-debug \
	--with-kernel-dir=/usr/src/linux-does-not-exist \
	--disable-strip 

CONFIGURE_VARS+= \
	ac_cv_file_$(call acvar,$(LINUX_DIR)/net/core/link_watch.c)=yes \
	ac_cv_lib_nl_nl_socket_modify_cb=no


define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		STRIP="/bin/true" \
		all install
endef

define Package/keepalived-devel/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/keepalived $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/genhash $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/keepalived
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/keepalived/keepalived.conf $(1)/etc/keepalived/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/keepalived.init $(1)/etc/init.d/keepalived
endef

$(eval $(call BuildPackage,keepalived-devel))
