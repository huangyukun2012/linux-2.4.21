#
# This file is subject to the terms and conditions of the GNU General Public
# License.  See the file "COPYING" in the main directory of this archive
# for more details.
#
# Copyright (C) 2000-2002 Silicon Graphics, Inc.  All Rights Reserved.
#
# Makefile for the sn kernel routines.

EXTRA_CFLAGS    := -DLITTLE_ENDIAN

ifdef CONFIG_IA64_SGI_SN2
EXTRA_CFLAGS    += -DSHUB_SWAP_WAR
endif

O_TARGET := sgiio.o
 
export-objs := hcl.o pci_dma.o

obj-y :=	stubs.o sgi_if.o xswitch.o klgraph_hack.o \
		hcl.o labelcl.o invent.o sgi_io_sim.o \
		klgraph_hack.o hcl_util.o cdl.o hubdev.o hubspc.o \
		alenlist.o pci.o pci_dma.o ate_utils.o \
		ifconfig_net.o io.o ifconfig_bus.o

obj-$(CONFIG_IA64_SGI_SN1)	+= sn1/snio.o
obj-$(CONFIG_IA64_SGI_SN2)	+= sn2/snio.o
obj-$(CONFIG_PCIBA)		+= pciba.o

subdir-$(CONFIG_IA64_SGI_SN1)	+= sn1
subdir-$(CONFIG_IA64_SGI_SN2)	+= sn2

include $(TOPDIR)/Rules.make
