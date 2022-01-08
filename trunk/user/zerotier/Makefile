SRC_NAME = ZeroTierOne-1.8.4
SRC_URL = https://github.com/zerotier/ZeroTierOne/archive/refs/tags/1.8.4.tar.gz
THISDIR = $(shell pwd)
FLAGS := DEFS=""
CFLAGS := -O3 -ffunction-sections -fdata-sections -fPIC
LDFLAGS := -Wl,--gc-sections,--as-needed

all: download_test extract_test
	$(MAKE) -j$(HOST_NCPU) -C $(SRC_NAME)

download_test:
	( if [ ! -f $(SRC_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).tar.gz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar zxf $(SRC_NAME).tar.gz ; \
		cd $(SRC_NAME); \
		sed -i 's/CFLAGS?=-O3 -fstack-protector -fPIE/CFLAGS?=-O3 -fstack-protector/g' $(THISDIR)/$(SRC_NAME)/make-linux.mk; \
		sed -i 's/CXXFLAGS?=-O3 -fstack-protector -fPIE/CXXFLAGS?=-O3 -fstack-protector/g' $(THISDIR)/$(SRC_NAME)/make-linux.mk; \
		sed -i 's/LDFLAGS=-pie -Wl,-z,relro,-z,now/LDFLAGS=-Wl,-z,relro,-z,now/g' $(THISDIR)/$(SRC_NAME)/make-linux.mk; \
	fi )

clean:
	# if [ -f $(SRC_NAME)/Makefile ] ; then \
	# 	$(MAKE) -C $(SRC_NAME) distclean ; \
	# fi ; \
	rm -rf $(SRC_NAME) $(SRC_NAME).tar.gz

romfs:
	$(ROMFSINST) -p +x  $(THISDIR)/$(SRC_NAME)/zerotier-one /usr/bin/zerotier-one
	$(ROMFSINST) -p +x  $(THISDIR)/zerotier.sh /usr/bin/zerotier.sh
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-cli
	ln -sf zerotier-one $(ROMFSDIR)/usr/bin/zerotier-idtool
	cp -fP $(CONFIG_CROSS_COMPILER_ROOT)/mipsel-linux-uclibc/sysroot/lib/libstdc++.so* $(ROMFSDIR)/lib

