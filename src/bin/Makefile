UNAME:=$(shell uname)

OBJDUMP=objdump
CFLAGS=-O0 -m32
LD=ld
ASM=nasm
TEXSRC_FILE=../ulixos.tex
ifeq ($(language), )
language=german
endif
TEXSRC_MODULE_FILE=../slip-mod_$(language).nw
EXTRACT_FILES=ulix.c ulix.h printf.c start.asm 

ifeq ($(UNAME),MacOS)
LD=/usr/cross/bin/i586-elf-ld
CC=/usr/cross/bin/i586-elf-gcc
OBJDUMP=/usr/cross/bin/i586-elf-objdump
qemu=qemu
endif

ifeq ($(UNAME),Linux)
CC=gcc
LDFLAGS=-m elf_i386
ASMFLAGS=-f elf
qemu=qemu-system-i386
endif

ifeq ($(UNAME),FreeBSD)
CC=/usr/local/bin/gcc48
LDFLAGS=-m elf_i386_fbsd
ASMFLAGS=-f elf32
qemu=qemu-system-i386
endif

all: build

build:	extract parse asm compile linking objdump mtools

extract: 
	notangle -L -Rulix.c $(TEXSRC_FILE) > ulix.c; true
	notangle -L -Rulix.h $(TEXSRC_FILE) > ulix.h
	notangle -L -Rprintf.c $(TEXSRC_FILE) > printf.c
	notangle -Rstart.asm $(TEXSRC_FILE) > start.asm
	notangle -Rulix.ld $(TEXSRC_FILE) > ulix.ld
	notangle -L -Rslip-mod.c $(TEXSRC_MODULE_FILE) > slip-mod.c
	notangle -L -Rslip-mod.h $(TEXSRC_MODULE_FILE) > slip-mod.h

parse:
	mv ulix.c ulix.c.pre
	./assembler-parser.py ulix.c.pre ulix.c
	sed -ie "s/SCRIPTBUILDDATE/`date`/" ulix.c

asm:
	mv slip-mod.c slip-mod.c.pre
	./assembler-parser.py slip-mod.c.pre slip-mod.c
	$(ASM) $(ASMFLAGS) -o start.o start.asm

compile:
	$(CC) $(CFLAGS) -fno-stack-protector -c -o printf.o printf.c
	$(CC) $(CFLAGS) -fno-stack-protector -std=c99 -g -O0 -nostdlib -nostdinc -fno-builtin -I./include -c -o slip-mod.o slip-mod.c 
	$(CC) $(CFLAGS) -fno-stack-protector -std=c99 -g -O0 -nostdlib -nostdinc -fno-builtin -I./include -c -o ulix.o -aux-info ulix.aux ulix.c

linking:
	$(LD) $(LDFLAGS) -T ulix.ld -o ulix.bin *.o

mtools:
	mcopy -o -i ulixboot.img  ulix.bin ::

objdump:
	$(OBJDUMP) -M intel -D ulix.bin > ulix.dump
	cat ulix.dump | grep -e '^[^ ]* <' | sed -e 's/<//' -e 's/>://' > ulix.sym

clean:
	rm -f ./*.o ./*.c ./*.h ./*.pre ./ulix.bin ./ulix.aux ./ulix.ce 
	rm -f ./ulix.dump* ./*asm ./*.objdump ./*sym

run: qemu-run

qemu-run:
	echo "Start first VM"
	$(qemu) -m 64 -fda ulixboot.img -fdb minixdata.img -serial file:/tmp/serial1 -serial unix:/tmp/serial2,server,nowait &
	echo "Start second VM"
	sleep 1
	$(qemu) -m 64 -fda ulixboot.img -fdb minixdata.img -serial file:/tmp/serial1 -serial unix:/tmp/serial2

	#
	# Test icmp with an other OS (like OpenBSD) 
	#
	# Attach Socket to sl0
	#
	# socat unix-connect:/tmp/serial2 pty
	# slattach -s 9600 -p slip /dev/pts/x	# x = pty number
	# ifconfig sl0 192.168.0.2/24 up

testrun:
	$(qemu) -m 64 -fda ulixboot.img -fdb minixdata.img -serial file:/dev/null -serial file:/dev/null
