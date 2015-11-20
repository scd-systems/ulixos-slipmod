UNAME:=$(shell uname)

ifeq ($(UNAME),Linux)
make=make
endif
ifeq ($(UNAME),FreeBSD)
make=gmake
endif
ifeq ($(language), )
language=german
subtitle:=für das Lehrbetriebssystem Ulix
endif
ifeq ($(language),english)
subtitle:=for Ulix teaching operating system
endif
workbook=slip-mod_$(language)
texfile=slip-mod
title:=Modul
titlepage=Titelpage
major_version:=$(shell grep 'define MAJOR_VERSION' ../$(workbook).nw | awk '{print $$3}')
minor_version:=$(shell grep 'define MINOR_VERSION' ../$(workbook).nw | awk '{print $$3}')
patch_version:=$(shell grep 'define PATCH_VERSION' ../$(workbook).nw | awk '{print $$3}')
date:=$(shell date)
pwd:=$(shell pwd)

pdf: 
	@noweave -autodefs c -index -delay ../${workbook}.nw > ${texfile}.tex
	@cp ${titlepage}.nw ${titlepage}.tex
	@perl -pi -e 's/__VERSION__/${major_version}\.${minor_version}\.p${patch_version}/g' ${titlepage}.tex
	@perl -pi -e 's/__DATE__/${date}/g' ${titlepage}.tex
	@perl -pi -e 's/__LANG__/${language}/g' ${titlepage}.tex
	@perl -pi -e 's/__TRANSLATE-TITLE__/${title}/g' ${titlepage}.tex
	@perl -pi -e 's/__TRANSLATE-SUBTITLE__/${subtitle}/g' ${titlepage}.tex
	@pdflatex Preamble.tex >/dev/null
	@makeindex Preamble.nlo -s nomencl.ist -o Preamble.nls >/dev/null
	@pdflatex Preamble.tex >/dev/null
	@pdflatex Preamble.tex >/dev/null
	@mv ./Preamble.pdf ./Slip-Mod.pdf
	@echo "PDF File: ${pwd}/Slip-Mod.pdf are successfully created"
html:
	@noweave -autodefs c -index -delay ../${workbook}.nw > ${texfile}.tex > /dev/null
	@cp ${titlepage}.nw ${titlepage}.tex
	@perl -pi -e 's/__VERSION__/${major_version}\.${minor_version}\.p${patch_version}/g' ${titlepage}.tex
	@perl -pi -e 's/__DATE__/${date}/g' ${titlepage}.tex
	@perl -pi -e 's/__LANG__/${language}/g' ${titlepage}.tex
	@perl -pi -e 's/__TRANSLATE-TITLE__/${title}/g' ${titlepage}.tex
	@perl -pi -e 's/__TRANSLATE-SUBTITLE__/${subtitle}/g' ${titlepage}.tex
	@htlatex Preamble.tex
	@makeindex Preamble.nlo -s nomencl.ist -o Preamble.nls >/dev/null
	@htlatex Preamble.tex

clean:
	@/bin/rm -f ./*.out ./*.lol ./*.lot ./*.out ./*.aux ./*.bak ./*.log ./*.nlo ./*.pdf ./*.lof ./*.toc ./*.nlg ./*.nls ./*.ilg ./*.ind
	@/bin/rm -f ./Preamble.pdf ./Slip-Mod.pdf ./${titlepage}.tex ./${texfile}.tex
