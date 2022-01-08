SRC_NAME = coremark-main
THISDIR = $(shell pwd)
SRC_URL = https://github.com/eembc/coremark/archive/refs/heads/main.zip

all: download_test extract_test
	$(MAKE) -C $(SRC_NAME) XCFLAGS="-DMULTITHREAD=8 -DUSE_PTHREAD" PORT_DIR=linux LFLAGS_END="-lpthread -lrt" compile

download_test:
	( if [ ! -f $(SRC_NAME).zip ]; then \
		wget -t5 --timeout=20 --no-check-certificate -O $(SRC_NAME).zip $(SRC_URL); \
	fi )

extract_test:
	( if [ ! -d $(SRC_NAME) ]; then \
		unzip -o $(SRC_NAME).zip; \
	fi )

clean:
	if [ -f $(SRC_NAME)/Makefile ] ; then \
		$(MAKE) -C $(SRC_NAME) PORT_DIR=linux clean ; \
	fi ; 

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/$(SRC_NAME)/coremark.exe /usr/bin/coremark
