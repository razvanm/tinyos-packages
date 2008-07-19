ARCH_TYPE ?= i386
INSTALL_DIR ?= usr
PACKAGES_TOP_DIR ?= ../../packages/
PACKAGES_DIR = $(PACKAGES_TOP_DIR)/$(ARCH_TYPE)

NAME = deputy-tinyos
VERSION = 1.1

FULL_NAME = $(NAME)-$(VERSION)

all: download package

download:
	wget http://tinyos.stanford.edu/tinyos/toolchain/repo/$(FULL_NAME).deb

move-it: 
	mkdir -p $(FULL_NAME);										\
	dpkg --extract $(FULL_NAME).deb $(FULL_NAME)/;							\
	mv $(FULL_NAME)/usr .										\

package: move-it
	mv usr $(FULL_NAME)/;										\
	rm -rf $(FULL_NAME)/changelog;									\
	mkdir -p $(FULL_NAME)/DEBIAN;									\
	cat $(NAME).control | sed 's/@version@/'$(VERSION)-`date +%Y%m%d`'/'				\
                           | sed 's/@architecture@/$(ARCH_TYPE)/'    					\
                           > $(FULL_NAME)/DEBIAN/control;                				\
	cat $(NAME).changelog | sed 's/@date@/'"`date -R`"'/g'                          \
                            >> $(FULL_NAME)/DEBIAN/changelog;                                 \
	dpkg-deb --build $(FULL_NAME) $(PACKAGES_DIR)/$(NAME)-$(VERSION).deb;				\

clean:
	rm -rf $(shell pwd)/$(NAME)/DEBIAN;

veryclean: clean
	rm -rf $(NAME)-$(VERSION);
