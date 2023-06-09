# dmenu - dynamic menu
# See LICENSE file for copyright and license details.

include config.mk

SRC = drw.c dmenu.c stest.c util.c
OBJ = $(SRC:.c=.o)

all: options dmenu stest

options:
	@echo dmenu build options:
	@echo "CFLAGS   = $(CFLAGS)"
	@echo "LDFLAGS  = $(LDFLAGS)"
	@echo "CC       = $(CC)"

.c.o:
	$(CC) -c $(CFLAGS) $<

config.h:
	cp config.def.h $@

$(OBJ): arg.h config.h config.mk drw.h

dmenu: dmenu.o drw.o util.o
	$(CC) -o $@ dmenu.o drw.o util.o $(LDFLAGS)

stest: stest.o
	$(CC) -o $@ stest.o $(LDFLAGS)

clean:
	rm -f dmenu stest config.h $(OBJ) dmenu-$(VERSION).tar.gz

dist: clean
	mkdir -p dmenu-$(VERSION)

	cp -r LICENSE Makefile PKGBUILD README.adoc arg.h config.def.h config.mk dmenu.1\
		  drw.h util.h scripts data stest.1 $(SRC)\
		  dmenu-$(VERSION)

	tar -cf dmenu-${VERSION}.tar -P dmenu-${VERSION}
	gzip dmenu-${VERSION}.tar

	rm -rf dmenu-$(VERSION)

install: all
	mkdir -p $(DESTDIR)$(PREFIX)/bin
	cp -f dmenu stest scripts/* data/* $(DESTDIR)$(PREFIX)/bin

	chmod 755 $(DESTDIR)$(PREFIX)/bin/stest
	chmod 755 $(DESTDIR)$(PREFIX)/bin/dmenu
	chmod 755 $(DESTDIR)$(PREFIX)/bin/dmenu_run
	chmod 755 $(DESTDIR)$(PREFIX)/bin/dmenu_path
	chmod 755 $(DESTDIR)$(PREFIX)/bin/dmenu_run_apps
	chmod 755 $(DESTDIR)$(PREFIX)/bin/dmenu_emoji_menu

	mkdir -p $(DESTDIR)$(MANPREFIX)/man1

	sed "s/VERSION/$(VERSION)/g" < dmenu.1 > $(DESTDIR)$(MANPREFIX)/man1/dmenu.1
	sed "s/VERSION/$(VERSION)/g" < stest.1 > $(DESTDIR)$(MANPREFIX)/man1/stest.1
	
	chmod 644 $(DESTDIR)$(MANPREFIX)/man1/dmenu.1
	chmod 644 $(DESTDIR)$(MANPREFIX)/man1/stest.1


uninstall:
	rm -f $(DESTDIR)$(PREFIX)/bin/dmenu\
		  $(DESTDIR)$(PREFIX)/bin/stest\
		  $(DESTDIR)$(PREFIX)/bin/dmenu_run\
		  $(DESTDIR)$(PREFIX)/bin/dmenu_path\
		  $(DESTDIR)$(PREFIX)/bin/dmenu_run_apps\
		  $(DESTDIR)$(PREFIX)/bin/dmenu_emoji_menu\
		  $(DESTDIR)$(PREFIX)/bin/emoji_data.json\
		  $(DESTDIR)$(MANPREFIX)/man1/dmenu.1\
		  $(DESTDIR)$(MANPREFIX)/man1/stest.1

.PHONY: all options clean dist install uninstall