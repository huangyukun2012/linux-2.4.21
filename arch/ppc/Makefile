#
# This file is included by the global makefile so that you can add your own
# architecture-specific flags and dependencies. Remember to do have actions
# for "archclean" and "archdep" for cleaning up and making dependencies for
# this architecture
#
# This file is subject to the terms and conditions of the GNU General Public
# License.  See the file "COPYING" in the main directory of this archive
# for more details.
#
# Copyright (C) 1994 by Linus Torvalds
# Changes for PPC by Gary Thomas
# Rewritten by Cort Dougan and Paul Mackerras
#

# Be sure to change PAGE_OFFSET in include/asm-ppc/page.h to match
KERNELLOAD	=0xc0000000

ifeq ($(shell uname -m),ppc)
CHECKS		= checks
endif

LINKFLAGS	= -T arch/ppc/vmlinux.lds -Ttext $(KERNELLOAD) -Bstatic
CPPFLAGS	:= $(CPPFLAGS) -I$(TOPDIR)/arch/$(ARCH)
AFLAGS		:= $(AFLAGS) -I$(TOPDIR)/arch/$(ARCH)
CFLAGS		:= $(CFLAGS) -I$(TOPDIR)/arch/$(ARCH) -fsigned-char \
		-msoft-float -pipe -ffixed-r2 -Wno-uninitialized \
		-mmultiple -mstring
HOSTCFLAGS	+= -I$(TOPDIR)/arch/$(ARCH)
CPP		= $(CC) -E $(CFLAGS)

ifdef CONFIG_4xx
CFLAGS := $(CFLAGS) -Wa,-m405
endif

ifdef CONFIG_8xx
CFLAGS := $(CFLAGS) -mcpu=860
endif

ifdef CONFIG_PPC64BRIDGE
CFLAGS := $(CFLAGS) -Wa,-mppc64bridge
endif

ifdef CONFIG_MORE_COMPILE_OPTIONS
# Use sed to remove the quotes.
  CFLAGS += $(shell echo $(CONFIG_COMPILE_OPTIONS) | sed -e 's/"//g')
endif

ifdef CONFIG_4xx
  HEAD := arch/ppc/kernel/head_4xx.o
else
  ifdef CONFIG_8xx
    HEAD := arch/ppc/kernel/head_8xx.o
  else
    HEAD := arch/ppc/kernel/head.o
    ifdef CONFIG_6xx
	HEAD += arch/ppc/kernel/idle_6xx.o
    endif
  endif
endif

ARCH_SUBDIRS = arch/ppc/kernel arch/ppc/platforms arch/ppc/mm arch/ppc/lib
SUBDIRS := $(SUBDIRS) $(ARCH_SUBDIRS)
CORE_FILES := arch/ppc/kernel/kernel.o arch/ppc/platforms/platform.o \
	arch/ppc/mm/mm.o arch/ppc/lib/lib.o $(CORE_FILES)

ifdef CONFIG_MATH_EMULATION
SUBDIRS += arch/ppc/math-emu
CORE_FILES += arch/ppc/math-emu/math-emu.o
endif

ifdef CONFIG_XMON
SUBDIRS += arch/ppc/xmon
CORE_FILES += arch/ppc/xmon/x.o
endif

MAKEBOOT = $(MAKE) -C arch/$(ARCH)/boot

ifdef CONFIG_8xx
SUBDIRS += arch/ppc/8xx_io
DRIVERS += arch/ppc/8xx_io/8xx_io.o
endif

ifdef CONFIG_8260
SUBDIRS += arch/ppc/8260_io
DRIVERS += arch/ppc/8260_io/8260_io.o
endif

ifdef CONFIG_APUS
SUBDIRS += arch/ppc/amiga
CORE_FILES += arch/ppc/amiga/amiga.o
endif

checks:
	@$(MAKE) -C arch/$(ARCH)/kernel checks

BOOT_TARGETS = zImage zImage.initrd znetboot znetboot.initrd

# All the instructions talk about "make bzImage".
bzImage: zImage

$(BOOT_TARGETS): $(CHECKS) vmlinux
	@$(MAKEBOOT) $@

%_config: arch/ppc/configs/%_defconfig
	rm -f .config arch/ppc/defconfig
	cp -f arch/ppc/configs/$(@:config=defconfig) arch/ppc/defconfig

archclean:
	rm -f arch/ppc/kernel/{mk_defs,ppc_defs.h,find_name,checks}
	@$(MAKEBOOT) clean

archmrproper:

archdep: scripts/mkdep
	$(MAKEBOOT) fastdep
