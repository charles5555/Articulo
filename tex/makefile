TOP      = $(shell pwd)

# OS Name (Linux or Darwin)
OSUPPER = $(shell uname -s 2>/dev/null | tr [:lower:] [:upper:])
OSLOWER = $(shell uname -s 2>/dev/null | tr [:upper:] [:lower:])

# Flags to detect 32-bit or 64-bit OS platform
OS_SIZE = $(shell uname -m | sed -e "s/i.86/32/" -e "s/x86_64/64/")
OS_ARCH = $(shell uname -m | sed -e "s/i386/i686/")

# Flags to detect either a Linux system (linux) or Mac OSX (darwin)
DARWIN = $(strip $(findstring DARWIN, $(OSUPPER)))

# OS-specific build flags
ifneq ($(DARWIN),)
    TEXFLAGS   := -interaction=batchmode
else
    TEXFLAGS   := -interaction=batchmode
endif

# COMPILERS OPTIONS
ifneq ($(DARWIN),)
  TEX          := /Library/TeX/texbin/pdflatex
else
  TEX          := /usr/bin/pdflatex
endif

#TEXFLAGS       += 

# FILES
MAINSOURCE := manuscript.tex
BASEMAINNAME = $(shell echo $(MAINSOURCE) | sed -e 's/\.tex//')
REMOVABLES = "$(BASEMAINNAME).aux" "$(BASEMAINNAME).bbl" "$(BASEMAINNAME).blg" \
				 "$(BASEMAINNAME).log" "$(BASEMAINNAME).out"
ifneq ($(DARWIN),)
    REMOVABLES   += "$(BASEMAINNAME).synctex.gz"
endif
SOURCES=$(shell ls *.tex | grep -v "manuscript.tex")
BIBSRC = bibliography.bib
BIBOBJ = manuscript.bbl

TARGET := manuscript.pdf

$(TARGET): $(SOURCES) $(BIBOBJ)
	$(TEX) $(TEXFLAGS) manuscript.tex

$(BIBOBJ) : $(BIBSRC)
	$(TEX) $(TEXFLAGS) $(MAINSOURCE)
	$(TEX) $(TEXFLAGS) $(MAINSOURCE)
	$(TEX) $(TEXFLAGS) $(MAINSOURCE)
	bibtex  $(BASEMAINNAME)
	$(TEX) $(TEXFLAGS) $(MAINSOURCE)

.PHONY: clean
clean:
	$(info CLEANING ...)
	@$(RM) -f $(TARGET) $(REMOVABLES)  2>/dev/null || true

.PHONY: help
help: ## Shows this help
	@echo '=================================================================================='
	@echo 'usage: make [target] ...'
	@echo '=================================================================================='
	@echo ''
	@echo 'targets:'
	@egrep -e '^(.+)\:\ .*##\ (.+)' ${MAKEFILE_LIST} | sed 's/:.*##/#/' | column -t -c 2 -s '#'
	@echo ''
	@echo '=================================================================================='


