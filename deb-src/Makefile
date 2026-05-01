PKG     := uc-tool-mx
BIN     := uc-tool
MAN     := uc-tool.1
SRCDIR  := $(PKG)
SCRIPT  := ../$(BIN)

.PHONY: all deb release prepare clean

all: deb

# Unsigned build for local testing.
deb: prepare
	cd $(SRCDIR) && debuild -uc -us

# Signed release build.  Override GPGKEY to use a specific key:
#   make release GPGKEY=ABCDEF1234
release: prepare
	cd $(SRCDIR) && debuild $(if $(GPGKEY),-k$(GPGKEY),)
	@echo ""
	@echo "Built: $$(ls -1 $(PKG)_*.deb 2>/dev/null | tail -1)"

prepare:
	install -m 0755 ../$(BIN) $(SRCDIR)/$(BIN)
	cp ../$(MAN)    $(SRCDIR)/$(MAN)
	cp ../README.md $(SRCDIR)/README.md

sign: release

clean:
	#rm -f $(PKG)_*.deb $(PKG)_*.buildinfo $(PKG)_*.changes
	rm $(PKG)_*.build
	rm -f $(SRCDIR)/$(BIN)
	rm -f $(SRCDIR)/$(MAN)
	rm -f $(SRCDIR)/README.md
	cd $(SRCDIR) && debian/rules clean 2>/dev/null || true
