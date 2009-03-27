ARCH_TYPE = all
INSTALL_DIR ?= usr

NAME = tinyos-optional-all
VERSION = 2.1

all: package

package: 
	mkdir -p debian/DEBIAN
	cat $(NAME).control | sed 's/@version@/'$(VERSION)-`date +%Y%m%d`'/'		\
                            | sed 's/@architecture@/$(ARCH_TYPE)/'                  	\
                            > debian/DEBIAN/control;                                	\
	cat $(NAME).changelog | sed 's/@date@/'"`date -R`"'/g'                          \
                            >> debian/DEBIAN/changelog;                                 \
	dpkg-deb --build debian ../../packages/$(ARCH_TYPE)/$(NAME)-$(VERSION).deb; 

clean:
	rm -rf debian
