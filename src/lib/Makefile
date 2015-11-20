UNAME:=$(shell uname)

NOWEBFILE=../ulixlib.nw

OBJDUMP=objdump
LD=ld
CCOPTIONS=-m32 -nostdlib -ffreestanding -fforce-addr -fomit-frame-pointer -fno-function-cse -nostartfiles -momit-leaf-frame-pointer
CCASMOPTIONS=-fverbose-asm -masm=intel
ifeq ($(language), )
language=german
endif
TEXSRC_MODULE_FILE=../slip-mod_$(language).nw

ifeq ($(UNAME),Linux) 
CC=gcc
LDOPTIONS=-m elf_i386 -static -s --pie
endif

ifeq ($(UNAME),FreeBSD)
CC=/usr/local/bin/gcc48
LDOPTIONS=-m elf_i386_fbsd -static -s --pie
endif

all: build

build: extract compile image

extract:
	notangle -Rulixlib.c < $(NOWEBFILE) > ulixlib.c
	notangle -Rulixlib.h < $(NOWEBFILE) > ulixlib.h
	notangle -L -Ripv4-tools.c $(TEXSRC_MODULE_FILE) > ipv4-tools.c
	notangle -L -Ripv4-tools.h $(TEXSRC_MODULE_FILE) > ipv4-tools.h

compile:
	$(CC) $(CCOPTIONS) $(CCTESTOPTIONS) -c ipv4-tools.c
	$(CC) $(CCOPTIONS) $(CCTESTOPTIONS) -c sh.c
	$(CC) $(CCOPTIONS) $(CCTESTOPTIONS) -c ulixlib.c
	
	# link it with linked script "linked.ld"
	$(LD) $(LDOPTIONS) -T process.ld --entry 0 -o sh sh.o ulixlib.o ipv4-tools.o

image:
ifeq ($(UNAME),Linux)
		sudo mount -o loop ../bin/minixdata.img ./mountpoint
		sudo rm ./mountpoint/sh ./mountpoint/etc/hosts ./mountpoint/etc/myhostname
		cp sh ./mountpoint/
		mkdir -p ./mountpoint/etc/
		cp ./etc_myhostname ./mountpoint/etc/myhostname
		cp ./etc_hosts ./mountpoint/etc/hosts
		sudo umount ./mountpoint
endif
ifeq ($(UNAME),FreeBSD)
		../bin/mfstool unlink ../bin/minixdata.img /etc/myhostname
		../bin/mfstool unlink ../bin/minixdata.img /etc/hosts
		../bin/mfstool unlink ../bin/minixdata.img ./sh
		../bin/mfstool add ../bin/minixdata.img ./sh /sh
		../bin/mfstool add ../bin/minixdata.img ./etc_myhostname /etc/myhostname
		../bin/mfstool add ../bin/minixdata.img ./etc_hosts /etc/hosts
endif

clean:
	rm -f ./*.o ./ipv4-tools.c ./ipv4-tools.h ./ulixlib.c ./ulixlib.h ./sh
