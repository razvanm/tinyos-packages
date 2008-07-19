INSTALL_DIR ?= usr

all:
	INSTALL_DIR=$(INSTALL_DIR) ./build-mspgcc install

clean:
	rm -rf archive build

packages:
	cd packages;		\
	make clean; make;	\	
