PKGDIR		?= ..
L4DIR		?= $(PKGDIR)/../..

EXAMPLE		= efi_hello
TARGET          = $(EXAMPLE).elfpic
INSTALL_TARGET  = $(EXAMPLE).efi $(TARGET)
DEL_ON_CLEAN    = $(INSTALL_TARGET)
LDSCRIPT        = $(OBJ_BASE)/lib/$(subst -,/,$(SYSTEM))/elf_$(EFI_ARCH)_efi.lds
SYSTEMS         = x86-plain amd64-plain arm64-plain
MODE            = sigma0
SRC_CC          = main.cc
REQUIRES_LIBS   = gnu-efi

include $(L4DIR)/mk/prog.mk

$(EXAMPLE).efi: $(EXAMPLE).elfpic
	$(VERBOSE)$(OBJCOPY) -j .text -j .sdata -j .data \
	           -j .dynamic -j .dynsym -j .rel \
	           -j .rela -j .reloc --subsystem=efi-app $< $@

EFI_ARCH-amd64  = x86_64
EFI_ARCH-x86    = ia32
EFI_ARCH-arm64  = aarch64
EFI_ARCH        = $(EFI_ARCH-$(ARCH))

OBJ           += $(OBJ_BASE)/lib/$(SYSTEM)/crt0-efi-$(EFI_ARCH).o
LDFLAGS_all_sigma0 =
CPPFLAGS       += -fshort-wchar -fPIC \
                  -I$(OBJ_BASE)/include/contrib/gnu-efi/$(EFI_ARCH)
LDFLAGS         = -nocrt $(if $(filter lld,$(BID_LD_TYPE)),-T,-dT) $(LDSCRIPT) \
		  -L$(OBJ_BASE)/lib/$(subst -,/,$(SYSTEM)) -nostdlib -shared \
		  -Bsymbolic --defsym=ImageBase=0 --defsym=EFI_SUBSYSTEM=0xa
