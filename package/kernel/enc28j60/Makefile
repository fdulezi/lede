include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=spi-enc28j60
PKG_RELEASE:=2

include $(INCLUDE_DIR)/package.mk

define KernelPackage/spi-enc28j60
  SUBMENU:=SPI Support
  TITLE:=SPI to Ethernet adapter enc28j60
  DEPENDS:=+kmod-spi-bcm2835	
  FILES:=$(PKG_BUILD_DIR)/enc28j60.ko
  AUTOLOAD:=$(call AutoLoad,50,enc28j60)
endef

define KernelPackage/spi-enc28j60/description
 Kernel module for enc28j60 SPI to Ethernet adapter
endef

EXTRA_KCONFIG:= \
	CONFIG_ENC28J60=m \
	CONFIG_ENC28J60_WRITEVERIFY=y
	# i do not know what CONFIG_ENC28J60_WRITEVERIFY is good
	
EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \

MAKE_OPTS:= \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	SUBDIRS="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	# the source came from here $(LINUX_DIR)/drivers/net/ethernet/microchip/*
	$(CP) ./src/* $(PKG_BUILD_DIR)/
	$(Build/Patch)
endef

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		$(MAKE_OPTS) \
		modules
endef

$(eval $(call KernelPackage,spi-enc28j60))
