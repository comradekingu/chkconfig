INSTALL= /usr/bin/install -c
INSTALL_PROGRAM= ${INSTALL}
INSTALL_DATA= ${INSTALL} -m 644
INSTALLNLSDIR=$(instroot)/usr/share/locale

MSGMERGE = msgmerge

NLSPACKAGE = chkconfig

CATALOGS = $(shell ls *.po | sed 's/\.po/\.mo/')

POTFILES  = ../chkconfig.c ../ntsysv.c ../leveldb.c

all: $(NLSPACKAGE).pot $(CATALOGS)

$(NLSPACKAGE).pot: $(POTFILES)
	xgettext --default-domain=$(NLSPACKAGE) \
       	  --add-comments --keyword=_ --keyword=N_ $(POTFILES)
	if cmp -s $(NLSPACKAGE).po $(NLSPACKAGE).pot; then \
	    rm -f $(NLSPACKAGE).po; \
	else \
	    mv $(NLSPACKAGE).po $(NLSPACKAGE).pot; \
	fi


update-po: Makefile
	$(MAKE) $(NLSPACKAGE).pot
	catalogs='$(CATALOGS)'; \
	for cat in $$catalogs; do \
		lang=`basename $$cat .po`; \
		if tupdate $(NLSPACKAGE).pot $$lang.po > $$lang.pot ; then \
			echo "tupdate of $$lang succeeded" ; \
		else \
			echo "tupdate of $$lang failed" ; \
			rm -f $$lang.pot ; \
		fi \
	done

clean:
	rm -f *mo $(NLSPACKAGE).pot

distclean: clean
	rm -f .depend Makefile

depend:

install:
	mkdir -p $(INSTALLNLSDIR)
	for n in $(CATALOGS); do \
	    l=`basename $$n .mo`; \
	    $(INSTALL) -m 755 -d $(INSTALLNLSDIR)/$$l; \
	    $(INSTALL) -m 755 -d $(INSTALLNLSDIR)/$$l/LC_MESSAGES; \
	    $(INSTALL) -m 644 $$n $(INSTALLNLSDIR)/$$l/LC_MESSAGES/$(NLSPACKAGE).mo; \
	done

%.mo: %.po
	msgfmt -o $@ $<