## Process this file with automake to produce Makefile.in

## Copyright (C) 2012 Måns Huldén

## This program is free software: you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation, either version 3 of the License, or
## (at your option) any later version.

## This program is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.

## You should have received a copy of the GNU General Public License
## along with this program.  If not, see <http://www.gnu.org/licenses/>.

ACLOCAL_AMFLAGS = -I m4

VERSION = $(PACKAGE_VERSION)
LIBRARY_RELEASE=0
LIBRARY_VERSION=9:16:9

# Local variables:
YACC = bison -d -t -v
LEX = flex -8
LEXCLEX = flex -8 --prefix=lexc
LEXIFACE = flex -8 --prefix=interface
LEXCMATRIX = flex -8 --prefix=cmatrix

# Linker flags:
AM_CFLAGS = -O3 -Wall -D_GNU_SOURCE -std=c99 -fvisibility=hidden -fPIC
AM_CPPFLAGS = 

# Main targets
lib_LTLIBRARIES = libfoma.la
bin_PROGRAMS = foma flookup cgflookup

# And their source files:
h_sources = fomalib.h fomalibconf.h
c_sources = apply.c coaccessible.c constructions.c \
			define.c determinize.c dynarray.c \
			extract.c flags.c int_stack.c io.c \
			lex.cmatrix.c lexc.h lex.lexc.c lex.yy.c lexcread.c \
			mem.c minimize.c \
			regex.c reverse.c rewrite.c \
			sigma.c spelling.c stringhash.c structures.c \
			topsort.c trie.c utf8.c

library_includedir = $(includedir)/foma
library_include_HEADERS = $(h_sources)

# Autotools variable bindings for the main targets:
libfoma_la_SOURCES = $(h_sources) $(c_sources)
#libfoma_la_LDFLAGS= -version-info $(LIBRARY_VERSION) -release $(LIBRARY_RELEASE)
libfoma_la_LDFLAGS= -version-info $(LIBRARY_VERSION) 

foma_SOURCES = foma.c foma.h iface.c lex.interface.c stack.c
foma_LDADD   = libfoma.la
foma_LDFLAGS = -lreadline -lz -ltermcap -static-libtool-libs

flookup_SOURCES = flookup.c
flookup_LDADD   = libfoma.la
flookup_LDFLAGS = -lz -static-libtool-libs

cgflookup_SOURCES = cgflookup.c
cgflookup_LDADD   = libfoma.la
cgflookup_LDFLAGS = -lz -static-libtool-libs

# Make sure the regex header file is built before it is included:
BUILT_SOURCES = regex.c regex.h

lex.yy.c: regex.l regex.h
	$(LEX) regex.l

lex.lexc.c: lexc.l
	$(LEXCLEX) $<

lex.interface.c: interface.l
	$(LEXIFACE) $<

lex.cmatrix.c: cmatrix.l
	$(LEXCMATRIX) $<

regex.c regex.h: regex.y
	$(YACC) --defines=$*.h --output=$*.c $<

EXTRA_DIST= regex.l regex.y \
			cmatrix.l interface.l \
			lexc.l
