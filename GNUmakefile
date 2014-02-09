# vim: ft=make ts=8 sts=2 sw=2 noet

PREFIX ?=	/usr/local
BINDIR ?=	$(PREFIX)/bin
MANDIR ?=	$(PREFIX)/share/man
MAN1DIR ?=	$(MANDIR)/man1

GZIPCMD?=	gzip
INSTALL_DATA?=	install -m 644
INSTALL_SCRIPT?=	install -m 755
RST2HTML?=$(call first_in_path,rst2html.py rst2html)

artifacts =	gtc.1.gz gtc

all: $(artifacts)

clean:
	$(RM) $(artifacts)

gtc: gtc.in
	$(INSTALL_SCRIPT) $< $@

%.html: %.rest
	$(RST2HTML) $< $@

%.1.html: %.1
	groff -mdoc -T html $< > $@

%.1.gz: %.1
	$(GZIPCMD) < $< > $@

install: gtc.1.gz
	@mkdir -p $(DESTDIR)$(BINDIR)
	@mkdir -p $(DESTDIR)$(MAN1DIR)
	@$(INSTALL_SCRIPT) gtc.in $(DESTDIR)$(BINDIR)/gtc
	@$(INSTALL_DATA) gtc.1.gz $(DESTDIR)$(MAN1DIR)/gtc.1.gz

define first_in_path
  $(firstword $(wildcard \
    $(foreach p,$(1),$(addsuffix /$(p),$(subst :, ,$(PATH)))) \
  ))
endef

.DEFAULT_GOAL := all

.PHONY: all
.PHONY: check
.PHONY: clean
.PHONY: install
