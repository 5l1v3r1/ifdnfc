IFDNFC_LIB = libifdnfc.$(DYN_LIB_EXT)
IFDNFC_BUNDLE = ifdnfc.bundle

lib_LTLIBRARIES = libifdnfc.la
libifdnfc_la_SOURCES = ifd-nfc.c atr.c
libifdnfc_la_LIBADD = $(LIBNFC_LIBS)
libifdnfc_la_CFLAGS = $(LIBNFC_CFLAGS) $(PCSC_CFLAGS)

bin_PROGRAMS = ifdnfc-activate
ifdnfc_activate_SOURCES = ifdnfc-activate.c
ifdnfc_activate_LDADD = $(LIBNFC_LIBS) $(PCSC_LIBS)
ifdnfc_activate_CFLAGS = $(LIBNFC_CFLAGS) $(PCSC_CFLAGS)

noinst_HEADERS = ifd-nfc.h atr.h

EXTRA_DIST = reader.conf.in

ifdnfcdropdir = $(DESTDIR)$(usbdropdir)/$(IFDNFC_BUNDLE)/Contents/$(BUNDLE_HOST)

install-data-local: install_ifdnfc_config

install-exec-local: install_ifdnfc install_ifdnfc_activate

install_ifdnfc: libifdnfc.la
	$(mkinstalldirs) $(ifdnfcdropdir)/
	cp .libs/$(IFDNFC_LIB) $(ifdnfcdropdir)/$(IFDNFC_LIB).$(VERSION)

install_ifdnfc_config:
	$(mkinstalldirs) $(ifdnfcdropdir)/
	cp Info.plist $(DESTDIR)$(usbdropdir)/$(IFDNFC_BUNDLE)/Contents/
	$(mkinstalldirs) $(DESTDIR)$(sysconfdir)/reader.conf.d
	sed "s#TARGETNAME#`awk '/IFDNFC_READER_NAME/ {print $$3}' $(srcdir)/ifd-nfc.h`#;\
	s#TARGETPATH#$(ifdnfcdropdir)/$(IFDNFC_LIB).$(VERSION)#"   $(srcdir)/reader.conf.in \
		> $(DESTDIR)$(sysconfdir)/reader.conf.d/ifdnfc

install_ifdnfc_activate: ifdnfc-activate
	$(mkinstalldirs) $(DESTDIR)$(bindir)
	$(LIBTOOL) --mode=install cp ifdnfc-activate $(DESTDIR)$(bindir)

uninstall: uninstall-libLTLIBRARIES
	rm -f $(DESTDIR)$(usbdropdir)/$(IFDNFC_BUNDLE)/Contents/Info.plist
	rm -f $(DESTDIR)$(usbdropdir)/$(IFDNFC_BUNDLE)/Contents/$(BUNDLE_HOST)/$(IFDNFC_LIB).$(VERSION)
	rm -f $(DESTDIR)$(bindir)/ifdnfc-activate
	rm -f $(DESTDIR)$(sysconfdir)/reader.conf.d/ifdnfc
