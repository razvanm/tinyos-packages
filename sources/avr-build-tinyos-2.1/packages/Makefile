ARCH_TYPE ?= i386
INSTALL_DIR ?= usr

AVRDUDE_NAME=avrdude
AVRDUDE_VERSION=5.4

AVR_LIBC_NAME=avr-libc
AVR_LIBC_VERSION=1.4.7

AVR_BINUTILS_NAME=avr-binutils
AVR_BINUTILS_VERSION=2.17

AVR_GCC_NAME=avr-gcc
AVR_GCC_VERSION=4.1.2

AVRDUDE=$(AVRDUDE_NAME)-$(AVRDUDE_VERSION)
AVR_LIBC=$(AVR_LIBC_NAME)-$(AVR_LIBC_VERSION)
AVR_BINUTILS=$(AVR_BINUTILS_NAME)-$(AVR_BINUTILS_VERSION)
AVR_GCC=$(AVR_GCC_NAME)-$(AVR_GCC_VERSION)
NAMES = $(AVRDUDE) $(AVR_LIBC) $(AVR_BINUTILS) $(AVR_GCC)

PACKAGES_DIR=../../../packages/$(ARCH_TYPE)
	
LOOP_LIST= "$(AVR_BINUTILS_NAME) $(AVR_BINUTILS_VERSION)"		\
	   "$(AVR_GCC_NAME) $(AVR_GCC_VERSION)"				\
	   "$(AVR_LIBC_NAME) $(AVR_LIBC_VERSION)"			\
	   "$(AVRDUDE_NAME) $(AVRDUDE_VERSION)"

all: remove-deps
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

remove-deps:
	rm -rf $(AVR_BINUTILS)/$(INSTALL_DIR)/lib/libiberty.a

clean:
	find . -name DEBIAN -type d | xargs rm -rf 

veryclean:
	rm -rf $(NAMES)
