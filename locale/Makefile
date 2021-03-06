#
# Makefile for PO merging and MO generation
#

POTFILE = app.pot
POFILES = $(shell find . -name '*.po')
MOFILES = $(patsubst %.po,%.mo,$(POFILES))
POXFILES = $(patsubst %.po,%.pox,$(POFILES))

%.mo: %.po
	msgfmt -o $@ $<

# Generate MO files from PO files
all-mo: $(MOFILES)

# Check for malformed strings
# TODO - enable endwhitespace, endpunc, puncspacing, options filters
%.pox: %.po
	msgfmt -c $<
	pofilter --nofuzzy -t variables -t blank -t urls -t emails -t long -t newlines \
		-t options -t printf -t validchars --gnome $< > $@
	! grep -q msgid $@

check: $(POXFILES)

# Merge PO files
update-po:
	for f in $(shell find ./ -name "*.po") ; do \
		msgmerge -N --backup=none -U $$f ${POTFILE} ; \
	done

# Unify duplicate translations
uniq-po:
	for f in $(shell find ./ -name "*.po") ; do \
		msguniq $$f -o $$f ; \
	done

# Remove all MO files
clean:
	-rm -f messages.mo
	find . \( -name "*.mo" -o -name "*.pox" \) -exec rm -f '{}' ';'
