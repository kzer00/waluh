include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-openclash
PKG_VERSION:=0.46.137
PKG_RELEASE:=70
PKG_MAINTAINER:=vernesong <https://github.com/vernesong/OpenClash>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/config
	config PACKAGE_kmod-inet-diag
	default y if PACKAGE_$(PKG_NAME)

	config PACKAGE_luci-compat
	default y if PACKAGE_$(PKG_NAME)

	config PACKAGE_kmod-nft-tproxy
	default y if PACKAGE_firewall4

	config PACKAGE_kmod-ipt-nat
	default y if ! PACKAGE_firewall4

	config PACKAGE_iptables-mod-tproxy
	default y if ! PACKAGE_firewall4

	config PACKAGE_iptables-mod-extra
	default y if ! PACKAGE_firewall4

	config PACKAGE_dnsmasq_full_ipset
	default y if ! PACKAGE_firewall4

	config PACKAGE_dnsmasq_full_nftset
	default y if PACKAGE_firewall4

	config PACKAGE_ipset
	default y if ! PACKAGE_firewall4
endef

define Package/$(PKG_NAME)
	CATEGORY:=LuCI
	SUBMENU:=3. Applications
	TITLE:=LuCI support for clash
	PKGARCH:=all
	DEPENDS:=+dnsmasq-full +bash +curl +ca-bundle +ip-full \
	+ruby +ruby-yaml +kmod-tun +unzip
	MAINTAINER:=vernesong
endef

define Package/$(PKG_NAME)/description
    A LuCI support for clash
endef

define Build/Prepare
	$(CP) $(CURDIR)/root $(PKG_BUILD_DIR)
	$(CP) $(CURDIR)/luasrc $(PKG_BUILD_DIR)
	$(foreach po,$(wildcard ${CURDIR}/po/zh-cn/*.po), \
		po2lmo $(po) $(PKG_BUILD_DIR)/$(patsubst %.po,%.lmo,$(notdir $(po)));)
	chmod 0755 $(PKG_BUILD_DIR)/root/etc/init.d/openclash
	chmod -R 0755 $(PKG_BUILD_DIR)/root/usr/share/openclash/
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/config
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/rule_provider
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/backup
	mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/core
	mkdir -p $(PKG_BUILD_DIR)/root/usr/share/openclash/backup
	cp -f "$(PKG_BUILD_DIR)/root/etc/config/openclash" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_rules.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_rules.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_rules_2.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_rules_2.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_hosts.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_hosts.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_fake_filter.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_fake_filter.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_domain_dns.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_domain_dns.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_domain_dns_policy.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_domain_dns_policy.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_fallback_filter.yaml" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_fallback_filter.yaml" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_sniffer.yaml" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_sniffer.yaml" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_localnetwork_ipv4.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_localnetwork_ipv4.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_localnetwork_ipv6.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_localnetwork_ipv6.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_chnroute_pass.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_chnroute_pass.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_chnroute6_pass.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_chnroute6_pass.list" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_firewall_rules.sh" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_firewall_rules.sh" >/dev/null 2>&1
	cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_overwrite.sh" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_overwrite.sh" >/dev/null 2>&1
	exit 0
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/$(PKG_NAME)/conffiles
endef

define Package/$(PKG_NAME)/preinst
#!/bin/sh
	if [ -f "/etc/config/openclash" ]; then
		cp -f "/etc/config/openclash" "/tmp/openclash.bak" >/dev/null 2>&1
		cp -rf "/etc/openclash" "/tmp/openclash" >/dev/null 2>&1
		cp -rf "/usr/share/openclash/ui/yacd" "/tmp/openclash_yacd" >/dev/null 2>&1
		cp -rf "/usr/share/openclash/ui/dashboard" "/tmp/openclash_dashboard" >/dev/null 2>&1
		cp -rf "/www/luci-static/resources/openclash/pac" "/tmp/pac" >/dev/null 2>&1
	fi
	exit 0
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
	exit 0
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
	uci -q set openclash.config.enable=0
	uci -q commit openclash
	[ -n "$(pidof clash)" ] && /etc/init.d/openclash stop 2>/dev/null
	cp -f "/etc/config/openclash" "/tmp/openclash.bak" >/dev/null 2>&1
	cp -rf "/etc/openclash" "/tmp/openclash" >/dev/null 2>&1
	cp -rf "/usr/share/openclash/ui/yacd" "/tmp/openclash_yacd" >/dev/null 2>&1
	cp -rf "/usr/share/openclash/ui/dashboard" "/tmp/openclash_dashboard" >/dev/null 2>&1
	cp -rf "/www/luci-static/resources/openclash/pac" "/tmp/pac" >/dev/null 2>&1
	exit 0
endef

define Package/$(PKG_NAME)/postrm
#!/bin/sh
	DEFAULT_DNSMASQ_CFGID="$$(uci -q show "dhcp.@dnsmasq[0]" | awk 'NR==1 {split($0, conf, /[.=]/); print conf[2]}' 2>/dev/null)"
	if [ -f "/tmp/etc/dnsmasq.conf.$DEFAULT_DNSMASQ_CFGID" ]; then
	   DNSMASQ_CONF_DIR="$$(awk -F '=' '/^conf-dir=/ {print $2}' "/tmp/etc/dnsmasq.conf.$DEFAULT_DNSMASQ_CFGID" 2>/dev/null)"
	else
	   DNSMASQ_CONF_DIR="/tmp/dnsmasq.d"
	fi
	DNSMASQ_CONF_DIR=$${DNSMASQ_CONF_DIR%*/}
	rm -rf /etc/openclash >/dev/null 2>&1
	rm -rf /etc/config/openclash >/dev/null 2>&1
	rm -rf /tmp/openclash.log >/dev/null 2>&1
	rm -rf /tmp/openclash_start.log >/dev/null 2>&1
	rm -rf /tmp/openclash_last_version >/dev/null 2>&1
	rm -rf /tmp/openclash_config.tmp >/dev/null 2>&1
	rm -rf /tmp/openclash.change >/dev/null 2>&1
	rm -rf /tmp/Proxy_Group >/dev/null 2>&1
	rm -rf /tmp/rules_name >/dev/null 2>&1
	rm -rf /tmp/rule_providers_name >/dev/null 2>&1
	rm -rf /tmp/clash_last_version >/dev/null 2>&1
	rm -rf /usr/share/openclash/backup >/dev/null 2>&1
	rm -rf ${DNSMASQ_CONF_DIR}/dnsmasq_openclash_custom_domain.conf >/dev/null 2>&1
	rm -rf ${DNSMASQ_CONF_DIR}/dnsmasq_openclash_chnroute_pass.conf >/dev/null 2>&1
	rm -rf ${DNSMASQ_CONF_DIR}/dnsmasq_openclash_chnroute6_pass.conf >/dev/null 2>&1
	rm -rf /tmp/dler* >/dev/null 2>&1
	rm -rf /tmp/etc/openclash >/dev/null 2>&1
	rm -rf /tmp/openclash_edit_file_name >/dev/null 2>&1
	rm -rf /tmp/openclash_announcement >/dev/null 2>&1
	rm -rf /usr/share/openclash/ui/xd >/dev/null 2>&1
	rm -rf /www/luci-static/resources/openclash/pac >/dev/null 2>&1
	sed -i '/OpenClash Append/,/OpenClash Append End/d' "/usr/lib/lua/luci/model/network.lua" >/dev/null 2>&1
	sed -i '/.*kB maximum content size*/c\HTTP_MAX_CONTENT      = 1024*100		-- 100 kB maximum content size' /usr/lib/lua/luci/http.lua >/dev/null 2>&1
	sed -i '/.*kB maximum content size*/c\export let HTTP_MAX_CONTENT = 1024*100;		// 100 kB maximum content size' /usr/share/ucode/luci/http.uc >/dev/null 2>&1
	uci -q delete firewall.openclash
	uci -q commit firewall
	[ -f "/etc/config/ucitrack" ] && {
	uci -q delete ucitrack.@openclash[-1]
	uci -q commit ucitrack
	}
	rm -rf /tmp/luci-*
	exit 0
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/*.*.lmo $(1)/usr/lib/lua/luci/i18n/
	$(CP) $(PKG_BUILD_DIR)/root/* $(1)/
	$(CP) $(PKG_BUILD_DIR)/luasrc/* $(1)/usr/lib/lua/luci/
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
