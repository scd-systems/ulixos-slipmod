UNAME:=$(shell uname)

ifeq ($(UNAME),Linux)
make=make
endif
ifeq ($(UNAME),FreeBSD)
make=gmake
endif

all: lib-build bin-build pdf-build

pdf:	pdf-build
pdf-build:	pdf-de

pdf-de: 
	$(make) -C doc language=german
pdf-en: 
	$(make) -C doc language=english
bin-build:
	$(make) -C bin

lib-build:
	$(make) -C lib

image: bin-build lib-build

qemu-run: bin-build lib-build
	$(make) -C bin qemu-run

clean:
	$(make) -C doc clean
	$(make) -C lib clean
	$(make) -C bin clean
