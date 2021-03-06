#
#

CONFIG_MODULE_SIG=n

KMOD_BASE := xx

CSCOPE_DIRS = .
CSCOPE_DIRS += ~/src/linux-stable/kernel
CSCOPE_DIRS += ~/src/linux-stable/block
CSCOPE_DIRS += ~/src/linux-stable/fs
CSCOPE_DIRS += ~/src/linux-stable/arch/x86
CSCOPE_DIRS += ~/src/linux-stable/mm
CSCOPE_DIRS += ~/src/linux-stable/include
CSCOPE_DIRS += ~/src/linux-stable/drivers
CSCOPE_DIRS += ~/src/linux-stable/lib
CSCOPE_DIRS += ~/src/linux-stable/tools
CSCOPE_DIRS += ~/src/glibc

obj-m += ${KMOD_BASE}.o

.PHONY: all clean cscope debug etags load tags unload

all: ${KMOD_BASE}.ko

clean:
	make -C /lib/modules/$(shell uname -r)/build M=$(CURDIR) clean

cscope: cscope.out

cscope.out: cscope.files
	cscope -b -u -k -q

cscope.files: Makefile
	find ${CSCOPE_DIRS} -type f -name \*.[ch] -o -name \*.cpp > $@.tmp
	if [ -n "${CSCOPE_EXCLUDE}" ] ; then \
		egrep -v ${CSCOPE_EXCLUDE} $@.tmp > $@.tmp2 ;\
		mv $@.tmp2 $@.tmp ;\
	fi
	mv $@.tmp $@

check:
	true

debug: CFLAGS+=-O0 -g
debug: ${KMOD_BASE}.ko

load: ${KMOD_BASE}.ko
	sudo insmod $<

tags etags: TAGS

TAGS: cscope.files
	cat cscope.files | xargs etags -a --members --output=$@

unload:
	sudo rmmod ${KMOD_BASE}

${KMOD_BASE}.ko:
	make -C /lib/modules/$(shell uname -r)/build M=$(CURDIR) modules
