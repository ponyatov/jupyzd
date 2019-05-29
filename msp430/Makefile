
APP    ?= blink

HW     ?= msp430g2

TARGET ?= msp430

CWD = $(CURDIR)
GZ  = $(CWD)/gz
TMP = $(CWD)/tmp
SRC = $(TMP)/src
GNU = $(CWD)/gnu_$(TARGET)

dirs:
	mkdir -p $(GZ) $(TMP) $(SRC) $(GNU)

BINUTILS_VER = 2.32

BINUTILS = binutils-$(BINUTILS_VER)

BINUTILS_GZ = $(BINUTILS).tar.xz

WGET = wget -c -P $(GZ) -q

gz: $(GZ)/$(BINUTILS_GZ)

$(SRC)/%/README: $(GZ)/%.tar.xz
	cd $(SRC) ; xzcat $< | tar -x && touch $@

CFG = configure --disable-nls

binutils: $(GNU)/bin/$(TARGET)-ld

CFG_BINUTILS += --target=$(TARGET)

$(GNU)/bin/$(TARGET)-ld: $(GZ)/$(BINUTILS_GZ) $(SRC)/$(BINUTILS)/README
	rm -rf $(TMP)/$(BINUTILS) ; mkdir $(TMP)/$(BINUTILS) ; cd $(TMP)/$(BINUTILS) ;\
	$(SRC)/$(BINUTILS)/$(CFG) $(CFG_BINUTILS)

$(GZ)/$(BINUTILS_GZ):
	$(WGET) https://mirror.tochlab.net/pub/gnu/binutils/$(BINUTILS_GZ)
