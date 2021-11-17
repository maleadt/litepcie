# Makefile for kernel module
KERNEL_VERSION:=$(shell uname -r)
KERNEL_PATH?=/lib/modules/$(KERNEL_VERSION)/build
ARCH?=$(shell uname -m)

obj-m = litepcie.o
litepcie-objs = main.o

#
# Get nv-p2p.h header file of the currently installed CUDA version.
# Try to get it based on available nvidia module version (just in case there are sources for couple of versions)
nv_version=$(shell /sbin/modinfo -F version -k $(KVER) nvidia 2>/dev/null)
nv_sources=$(shell /bin/ls -d /usr/src/nvidia-$(nv_version)/ 2>/dev/null)
ifneq ($(shell test -d "$(nv_sources)" && echo "true" || echo "" ),)
NV_P2P_H=$(shell /bin/ls -1 $(nv_sources)/nvidia/nv-p2p.h 2>/dev/null | tail -1)
else
NV_P2P_H=$(shell /bin/ls -1 /usr/src/nvidia-*/nvidia/nv-p2p.h 2>/dev/null | tail -1)
endif


all: litepcie.ko

litepcie.ko: main.c
	make -C $(KERNEL_PATH) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(shell pwd) modules

clean:
	make -C $(KERNEL_PATH) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(shell pwd) clean
	rm -f *~
