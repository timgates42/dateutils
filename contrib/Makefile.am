### Makefile.am

AM_CPPFLAGS = -D_POSIX_C_SOURCE=200112L -D_XOPEN_SOURCE=600 -D_BSD_SOURCE
AM_LDFLAGS =
EXTRA_DIST =

MEX_LDFLAGS = -module -shared -avoid-version -shrext ".mex"

mex_LTLIBRARIES =
mex_DATA =
EXTRA_DIST += $(mex_DATA)

if BUILD_MEXCLI
mexdir = $(MATLABPATH)/dateutils
octlab_CFLAGS = -DMATLAB_MEX_FILE $(matlab_CFLAGS)
else  !BUILD_MEXCLI

## backup plan is to use octave to build the mex file
if BUILD_OCTCLI
mexdir = $(OCTAVEPATH)/dateutils
octlab_CFLAGS = -DOCTAVE_MEX_FILE $(octave_CFLAGS)

else  !BUILD_OCTCLI
## no idea what we're doing here
mexdir = $(pkglibexecdir)

endif  !BUILD_OCTCLI
endif  !BUILD_MEXCLI


mex_LTLIBRARIES += tzconv.la
mex_DATA += tzconv.m
tzconv_la_LDFLAGS = $(AM_LDFLAGS)
tzconv_la_LDFLAGS += $(MEX_LDFLAGS)
tzconv_la_LDFLAGS += $(XCCLDFLAGS)
tzconv_la_CPPFLAGS = $(AM_CPPFLAGS)
tzconv_la_CPPFLAGS += $(octlab_CFLAGS)
tzconv_la_CPPFLAGS += -I$(top_srcdir)/lib
tzconv_la_CPPFLAGS += -I$(top_builddir)/lib
tzconv_la_CPPFLAGS += -DDECLF=extern -DDEFUN=
tzconv_la_SOURCES = tzconv.c
tzconv_la_SOURCES += tzraw.c tzraw.h
tzconv_la_SOURCES += leaps.c leaps.h


## get around subdir-object bug in conjunction with distclean in automake 1.14+
tzraw.c tzraw.h:
	$(LN_S) $(top_srcdir)/lib/$@ $@

leaps.c leaps.h:
	$(LN_S) $(top_srcdir)/lib/$@ $@

install-data-hook:
	-cd $(DESTDIR)$(mexdir) && mv -f tzconv.mex tzconv.$(MEXEXT)

uninstall-hook:
	-$(RM) -- $(DESTDIR)$(mexdir)/tzconv.$(MEXEXT)

# 
# Help the developers get nice post-processed source files

## Create preprocessor output (debugging purposes only)
.c.i:
	$(COMPILE) -E -o $@ $<

## Create assembler output (debugging purposes only)
.c.s:
	$(COMPILE) -S -c $(AM_CFLAGS) $<
