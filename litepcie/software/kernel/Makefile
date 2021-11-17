# Makefile for kernel module
KERNEL_VERSION:=$(shell uname -r)
KERNEL_PATH?=/lib/modules/$(KERNEL_VERSION)/build
ARCH?=$(shell uname -m)

obj-m = litepcie.o
litepcie-objs = main.o

# discover the sources of the NVIDIA driver based on the loaded module
# (just in case there are sources for couple of versions)
nv_version=$(shell /sbin/modinfo -F version -k $(KERNEL_VERSION) nvidia 2>/dev/null)
nv_sources=$(shell /bin/ls -d /usr/src/nvidia-$(nv_version)/ 2>/dev/null)
ifneq ($(shell test -d "$(nv_sources)" && echo "true" || echo "" ),)
	NVIDIA_SRC_DIR ?= $(shell find "$(nv_sources)" -name "nv-p2p.h"|head -1|xargs dirname || echo "NVIDIA_DRIVER_MISSING")
else
	NVIDIA_SRC_DIR ?= $(shell find /usr/src/nvidia-* -name "nv-p2p.h"|head -1|xargs dirname || echo "NVIDIA_DRIVER_MISSING")
endif
ccflags-y += -I$(NVIDIA_SRC_DIR)

all: litepcie.ko

litepcie.ko: main.c
	make -C $(KERNEL_PATH) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(shell pwd) modules

clean:
	make -C $(KERNEL_PATH) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(shell pwd) clean
	rm -f *~
