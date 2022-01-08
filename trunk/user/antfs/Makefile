SRC_NAME = antfs-07.19

all:
ifeq ($(CONFIG_WITHOUT_KERNEL),y)
	@echo "antfs would not build!"
else
	$(MAKE) -C $(SRC_NAME)
	cp -f $(SRC_NAME)/antfs.ko .
endif

romfs:

clean:
	$(MAKE) -C $(SRC_NAME) clean
	rm -f *.ko


