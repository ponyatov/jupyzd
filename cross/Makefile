
APP    ?= blink

HW     ?= msp430g2

-include  hw/$(HW)
-include cpu/$(CPU)

CWD = $(CURDIR)
GZ  = $(HOME)/gz
TMP = $(CWD)/tmp
SRC = $(TMP)/src
GNU = $(CWD)/$(TARGET)

all: dirs gz binutils gdb

dirs:
	mkdir -p $(GZ) $(TMP) $(SRC) $(GNU)

BINUTILS_VER = 2.32
GDB_VER      = 8.3

BINUTILS = binutils-$(BINUTILS_VER)
GDB      = gdb-$(GDB_VER)

BINUTILS_GZ = $(BINUTILS).tar.xz
GDB_GZ      = $(GDB).tar.xz

WGET = wget -c -P $(GZ) -q

gz: $(GZ)/$(BINUTILS_GZ) $(GZ)/$(GDB_GZ)

$(SRC)/%/README: $(GZ)/%.tar.xz
	cd $(SRC) ; xzcat $< | tar -x && touch $@

XPATH = PATH=$(GNU)/bin:$(PATH)

CFG = configure --disable-nls --prefix=$(GNU)

MAKE = $(XPATH) make

CPU_CORES = $(shell grep processor /proc/cpuinfo|wc -l)

MAKEJ = $(MAKE) -j$(CPU_CORES)

binutils: $(GNU)/bin/$(TARGET)-ld

CFG_BINUTILS += --target=$(TARGET)

$(GNU)/bin/$(TARGET)-ld: $(GZ)/$(BINUTILS_GZ) $(SRC)/$(BINUTILS)/README
	rm -rf $(TMP)/$(BINUTILS) ; mkdir $(TMP)/$(BINUTILS) ; cd $(TMP)/$(BINUTILS) ;\
	$(XPATH) $(SRC)/$(BINUTILS)/$(CFG) $(CFG_BINUTILS) && $(MAKEJ) && $(MAKE) install-strip

$(GZ)/$(BINUTILS_GZ):
	$(WGET) https://mirror.tochlab.net/pub/gnu/binutils/$(BINUTILS_GZ)

gdb: $(GNU)/bin/$(TARGET)-gdb

CFG_GDB += $(CFG_BINUTILS) --enable-python

$(GNU)/bin/$(TARGET)-gdb: $(GZ)/$(GDB_GZ) $(SRC)/$(GDB)/README
	rm -rf $(TMP)/$(GDB) ; mkdir $(TMP)/$(GDB) ; cd $(TMP)/$(GDB) ;\
	$(XPATH) $(SRC)/$(GDB)/$(CFG) $(CFG_GDB) && $(MAKEJ) && $(MAKE) install

$(GZ)/$(GDB_GZ):
	$(WGET) http://ftp.gnu.org/gnu/gdb/$(GDB_GZ)
