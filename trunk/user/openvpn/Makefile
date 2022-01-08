SRC_NAME = openvpn-2.5.5
SRC_URL = https://github.com/OpenVPN/openvpn/archive/refs/tags/v2.5.5.tar.gz
ARCHIVE_NAME = v2.5.5
CFLAGS += -ffunction-sections -fdata-sections
LDFLAGS += -Wl,--gc-sections

all: download_test extract_test config_test
	$(MAKE) -C $(SRC_NAME)

download_test:
	( if [ ! -f $(ARCHIVE_NAME).tar.gz ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(ARCHIVE_NAME).tar.gz $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		tar -zxf $(ARCHIVE_NAME).tar.gz; \
	fi )

config_test:
	( if [ -f ./config_done ]; then \
		echo "the same configuration"; \
	else \
		make configure && touch config_done; \
	fi )

configure:
	( cd $(SRC_NAME) ; \
	autoreconf -vi ; \
	IFCONFIG=/sbin/ifconfig \
	ROUTE=/sbin/route \
	NETSTAT=/bin/netstat \
	./configure \
		--prefix=/usr \
		--disable-debug \
		--disable-plugins \
		--disable-pkcs11 \
		--disable-ofb-cfb \
		--disable-selinux \
		--disable-systemd \
		--disable-iproute2 \
		--disable-management \
		--disable-socks \
		--enable-small \
		--enable-multi \
		--enable-server \
		--enable-multihome \
		--enable-pf \
		--enable-fragment \
		--enable-http-proxy \
		--enable-def-auth \
		--enable-ssl \
		--enable-crypto \
		--enable-lzo \
		--with-crypto-library=openssl \
		OPENSSL_CRYPTO_CFLAGS="-I$(STAGEDIR)/include" \
		OPENSSL_CRYPTO_LIBS="-L$(STAGEDIR)/lib -lcrypto" \
		OPENSSL_SSL_CFLAGS="-I$(STAGEDIR)/include" \
		OPENSSL_SSL_LIBS="-L$(STAGEDIR)/lib -lssl -lcrypto" \
		LZO_CFLAGS="-I$(STAGEDIR)/include" \
		LZO_LIBS="-L$(STAGEDIR)/lib -llzo2" \
		--with-sysroot=$(STAGEDIR) \
		--host=$(HOST_TARGET) \
		--build=$(HOST_BUILD) ; \
	)

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) distclean ; \
	fi ; \
	rm -f config_done
	rm -rf $(SRC_NAME) $(ARCHIVE_NAME).tar.gz

romfs:
	$(ROMFSINST) $(SRC_NAME)/src/openvpn/openvpn /usr/sbin/openvpn
	$(ROMFSINST) -p +x $(SRC_NAME)/openvpn-cert.sh /usr/bin/openvpn-cert.sh
