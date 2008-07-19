ARCH_TYPE ?= i386
INSTALL_DIR ?= usr
PACKAGES_TOP_DIR ?= ../../../packages/
PACKAGES_DIR = $(PACKAGES_TOP_DIR)/$(ARCH_TYPE)

MSP_LIBC_NAME=msp430-libc
MSP_LIBC_VERSION=20060801cvs

MSP_BINUTILS_NAME=msp430-binutils
MSP_BINUTILS_VERSION=2.17

MSP_GCC_NAME=msp430-gcc
MSP_GCC_VERSION=3.2.3

MSP_LIBC=$(MSP_LIBC_NAME)-$(MSP_LIBC_VERSION)
MSP_BINUTILS=$(MSP_BINUTILS_NAME)-$(MSP_BINUTILS_VERSION)
MSP_GCC=$(MSP_GCC_NAME)-$(MSP_GCC_VERSION)
NAMES = $(MSP_LIBC) $(MSP_BINUTILS) $(MSP_GCC)

	
LOOP_LIST= "$(MSP_BINUTILS_NAME) $(MSP_BINUTILS_VERSION)"		\
	   "$(MSP_GCC_NAME) $(MSP_GCC_VERSION)"				\
	   "$(MSP_LIBC_NAME) $(MSP_LIBC_VERSION)"

all: 
	for i in $(LOOP_LIST);								\
	do										\
		set -- $$i;								\
		mkdir -p $$1-$$2/DEBIAN/;						\
		cat control-files/$$1.control | sed 's/@version@/'$$2-`date +%Y%m%d`'/'	\
					      | sed 's/@architecture@/$(ARCH_TYPE)/'	\
					      > $$1-$$2/DEBIAN/control;			\
		dpkg-deb --build $$1-$$2 $(PACKAGES_DIR)/$$1-tinyos-$$2.deb;		\
	done

packages: all

remove-$(MSP_BINUTILS)-deps:
	find $(INSTALL_DIR) -type f > $(MSP_BINUTILS).files
	rm -rf $(MSP_BINUTILS)
	mkdir -p $(MSP_BINUTILS)
	cp -R $(INSTALL_DIR) $(MSP_BINUTILS)/
	rm -rf  $(MSP_BINUTILS)/$(INSTALL_DIR)/info
	rm -rf $(MSP_BINUTILS)/$(INSTALL_DIR)/lib/libiberty.a
	rm -rf $(MSP_BINUTILS)/$(INSTALL_DIR)/share/locale
	rm -rf $(MSP_BINUTILS)/$(INSTALL_DIR)/man/man7

remove-$(MSP_GCC)-deps:
	cat $(MSP_BINUTILS).files | xargs rm -rf
	find $(INSTALL_DIR) -empty | xargs rm -rf
	find $(INSTALL_DIR) -type f > $(MSP_GCC).files
	rm -rf $(MSP_GCC)
	mkdir -p $(MSP_GCC)
	cp -R $(INSTALL_DIR) $(MSP_GCC)/
	cp -R $(MSP_BINUTILS)/* .
	rm -rf $(MSP_GCC)/$(INSTALL_DIR)/man/man7

remove-$(MSP_LIBC)-deps:
	cat $(MSP_BINUTILS).files | xargs rm -rf
	cat $(MSP_GCC).files | xargs rm -rf
	find $(INSTALL_DIR) -empty | xargs rm -rf
	find $(INSTALL_DIR) -type f > $(MSP_LIBC).files
	rm -rf $(MSP_LIBC)
	mkdir -p $(MSP_LIBC)
	cp -R $(INSTALL_DIR) $(MSP_LIBC)/
	cp -R $(MSP_BINUTILS)/* .
	cp -R $(MSP_GCC)/* .

clean:
	find . -name DEBIAN -type d | xargs rm -rf 

veryclean: clean
	rm -rf $(NAMES)
	rm -rf *.files
	rm -rf $(INSTALL_DIR)
