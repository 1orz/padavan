THISDIR = $(shell pwd)

SRC = https://down.cloudorz.com/Software/Xray/Xray-linux-mips32le.zip
SRC_R = https://down.cloudorz.com/Software/XrayR/XrayR-linux-mips32le.zip


all:download_binary build_extract

download_binary:
	curl --create-dirs -L $(SRC) -o $(THISDIR)/Xray-linux-mips32le.zip
	#curl --create-dirs -L $(SRC_R) -o $(THISDIR)/XrayR-linux-mips32le.zip

build_extract:
	mkdir -p $(THISDIR)/bin
	unzip -d bin/ -o Xray-linux-mips32le.zip
	#unzip -d bin/ -o XrayR-linux-mips32le.zip

clean:
	rm -rf $(THISDIR)/bin *.zip

romfs:
	$(ROMFSINST) -p +x $(THISDIR)/bin/xray /usr/bin/xray
	#$(ROMFSINST) -p +x $(THISDIR)/bin/XrayR /usr/bin/xrayr
	ln -sf xray $(ROMFSDIR)/usr/bin/v2ray
