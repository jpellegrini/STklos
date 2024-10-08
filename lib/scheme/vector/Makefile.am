# Makefile for the SRFI 160
#
# Copyright © 2022 Jeronimo Pellegrini <j_p@aleph0.info>
# Based on the SRFI Makefiles by Erick Gallesio.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA 02111-1307,
# USA.
#
#           Author: Jeronimo Pellegrini [j_p@aleph0.info]
#    Creation date: 20-Jun-2022 09:24 (jpellegrini)

#-======================================================================


COMP ?= ../../../utils/tmpcomp
STKLOS_BINARY ?= ../../../src/stklos
STKLOS_FLAGS   = --no-init-file --case-sensitive
SO = @SH_SUFFIX@

# ORGANIZATION OF (SCHEME VECTOR @) IMPLEMENTATION:
#
# - base.c, base.stk: implementation of the base library,
#   (scheme vector base), where there's also much code that is
#   used by the specific s8, u8, etc, libraries.
#
# - s8.stk, u8,stk, ..., c128.stk: the specific libraries
#   (scheme vector s8), and so on. They have thin wrappers over
#   procedures defined in base.c
#
# - @.stk: the non-standard (scheme vector @) library. It is useful
#   for importing everything.
#
# the s8.stk, etc files are generated (with sed) from the file
# tagvector-template.stk.


SRC_STK      = @.stk

SRC_GEN_OSTK= s8.ostk   \
              u8.ostk   \
              s16.ostk  \
              u16.ostk  \
              s32.ostk  \
              u32.ostk  \
              s64.ostk  \
              u64.ostk  \
              f32.ostk  \
              f64.ostk  \
              c64.ostk  \
              c128.ostk

SRC_OSTK = $(SRC_GEN_OSTK) @.ostk

SRC_C     = base.c
SRC_C_STK = base.stk
SRC_SHOBJ = base.$(SO)

srfi_OBJS = $(SRC_OSTK) $(SRC_SHOBJ)

srfi_interm = s8.stk u8.stk s16.stk u16.stk \
	      s32.stk u32.stk s64.stk u64.stk \
	      f32.stk f64.stk c64.stk c128.stk


DOCDB  = ../../DOCDB
BASEDIR= ../../..
SOURCES= $(SRC_C) $(SRC_STK) $(SRC_C_STK)


#======================================================================
srfidir       = @SCMDIR@/scheme/vector
srfilibdir    = @EXECDIR@/scheme/vector
srfi_DATA     = $(SRC_OSTK)
srfilib_DATA  = $(SRC_SHOBJ)
srfi_sources  = $(SRC_STK)
#======================================================================
SUFFIXES = .stk .ostk .stk -incl.c .$(SO) .c

# The procedures defined are exactly the same for all data types,
# only the type changes. So we generate them from a template.
#
# We use a multiple-target rule, which calls sed to change the
# string "TAG" in the template into s8, u8, etc, writing it to
# a new file "s8.stk", "u8.stk" etc.
s8.stk  u8.stk  s16.stk u16.stk \
s32.stk u32.stk s64.stk u64.stk \
f32.stk f64.stk c64.stk c128.stk: tagvector-template.stk base.$(SO)
	sed "s/TAG/$*/g"   tagvector-template.stk > $@

#======================================================================

.stk.ostk:
	$(COMP) -o $*.ostk $*.stk

.stk-incl.c:
	$(COMP) -C -o $*-incl.c $*.stk

.c.$(SO) :
	@CC@ @CFLAGS@ @CPPFLAGS@ @STKCFLAGS@ @SH_COMP_FLAGS@ \
			-I../../../src @GCINC@ @GMPINC@ -c -o $*.o $*.c
	@SH_LOADER@ @SH_LOAD_FLAGS@ @LDFLAGS@ -o $*.$(SO) $*.o @DLLIBS@
	@/bin/rm -f $*.o

#======================================================================
# Dependencies

../../scheme/comparator.ostk:
	(cd ../../scheme && $(MAKE) comparator.ostk)

../../srfi/4.ostk:
	(cd ../../srfi && $(MAKE) 4.ostk)

base.$(SO): ../../srfi/4.ostk ../../scheme/comparator.ostk base-incl.c base.c

@.stk: $(SRC_GEN_OSTK)


#======================================================================

install-sources:
	mkdir -p $(srfidir)
	cp $(srfi_sources) $(srfi_interm) $(srfidir)

# we also clean the TAG.stk files -- $(srfi-interm) --, which are NOT
# srfi_OBJS (should not be installed with other objects):
clean:
	rm -f $(srfi_OBJS) *-incl.c *~

distclean: clean
	/bin/rm -f Makefile $(srfi_interm)

uninstall-hook:
	(cd $(srfidir) && rm -f $(srfi_sources) $(srfi_interm)) || true
	rmdir $(srfidir) || true
##
## # no docs...
## # doc:
## # 	$(STKLOS_BINARY) $(STKLOS_FLAGS) -b ../../src/boot.img \
## # 		-f ../../doc/extract-doc $(SOURCES) >> $(DOCDB)
