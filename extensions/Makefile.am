#
#  Makefile 	-- Makefile for the STklos extensions
#
# Copyright © 2021-2025 Erick Gallesio <eg@stklos.net>
#
#           Author: Erick Gallesio [eg@unice.fr]
#    Creation date:  6-Jul-2021 11:53
#


subdirs= curl fuse gtklos

all:
	@echo "***"
	@echo "*** The extensions must be built in their own directory"
	@echo "***"

clean-am:
	for d in $(subdirs) ;do (cd $$d && $(MAKE) $@) ;done
	rm -f *~

distclean-am: clean-am
	for d in $(subdirs) ;do (cd $$d && $(MAKE) distclean) ;done
	rm -f Makefile
