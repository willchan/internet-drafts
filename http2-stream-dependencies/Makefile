# Base directory for I-D
ID_BASE = ..

saxpath = "/home/willchan/local/bin/saxon9he.jar"
saxon = java -classpath $(saxpath) net.sf.saxon.Transform -l
rfc2629dir = $(ID_BASE)/tools/rfc2629xslt
stylesheet = $(rfc2629dir)/rfc2629.xslt

NAME = chan
TITLE = ${NAME}-$(shell basename ${CURDIR})
VERSION = 00

TARGET_XML = draft-$(TITLE)-$(VERSION).xml
PREV_TXT = draft-$(TITLE)-$(shell printf "%.2d" `echo ${VERSION}-1 | bc`).txt
NEXT_XML = draft-$(TITLE)-$(shell printf "%.2d" `echo ${VERSION}+1 | bc`).xml

TARGET_HTML = $(TARGET_XML:.xml=.html)
TARGET_TXT = $(TARGET_XML:.xml=.txt)
TARGETS = $(TARGET_HTML) \
          $(TARGET_TXT)

INTERMEDIATE_REDXML = $(TARGET_XML:.xml=.redxml)
INTERMEDIATES = $(INTERMEDIATE_REDXML)

.INTERMEDIATE: $(INTERMEDIATES)

.PHONY: all
all: $(TARGETS)

.PHONY: clean
clean:
	rm -f $(TARGETS) $(INTERMEDIATES)

.PHONY: next
next:
	cp $(TARGET_XML) $(NEXT_XML)

.PHONY: diff
diff:
	rfcdiff $(PREV_TXT) $(TARGET_TXT)

.PHONY: idnits
idnits: $(TARGET_TXT)
	idnits $<

%.html: %.xml
	$(saxon) $< $(stylesheet) > $@

reduction := $(rfc2629dir)/clean-for-DTD.xslt
%.redxml: %.xml $(reduction)
	$(saxon) $< $(reduction) > $@

%.txt: %.redxml
	xml2rfc $< $@
