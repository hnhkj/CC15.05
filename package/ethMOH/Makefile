
include $(TOPDIR)/rules.mk

PKG_NAME:=ipRec
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define Package/ethMOH
  TITLE:=ethMOH board support package
  HIDDEN:=1
  DEPENDS:=@TARGET_ramips_mt7688_LinkIt7688 \
	+gdbserver +curl \
	+uhttpd \
	+luci \
	+cgi-io \
	+mountd	\
	+kmod-fs-vfat +kmod-fs-exfat \
	+kmod-nls-base +kmod-nls-cp437 +kmod-nls-iso8859-1 \
	+kmod-nls-iso8859-15 +kmod-nls-iso8859-2 +kmod-nls-utf8 \
	+kmod-sdhci-mt7620 +kmod-usb-storage \
	+kmod-sound-core +kmod-sound-mtk +madplay-alsa +alsa-utils \
    +mtk-sdk-wifi

  CATEGORY:=Base system
  DEFAULT:=y
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
endef

define Build/Compile
endef

define Package/ethMOH/install
	$(INSTALL_DIR) $(1)/sbin
	
	$(CP) ./files/* $(1)
endef

$(eval $(call BuildPackage,ethMOH))
