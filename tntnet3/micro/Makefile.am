ACLOCAL_AMFLAGS = -I m4
AUTOMAKE_OPTIONS = subdir-objects

SRC_PATH := src/
SRC_PUBLIC := src/public

models := \
	src/api/models/index.ecpp

controllers := \
	src/api/controllers/index.ecpp

actions := \
	src/api/actions/myaction.ecpp

bin_PROGRAMS = MyApi

noinst_HEADERS := \
	src/actionreply.h \
	src/action.ecpp \
	src/configuration.h \
	src/noty.h \
	src/session.ecpp \
	$(models)

ecppSources = \
	src/actionmain.ecpp \
	src/api/jsons/example.ecpp \
	src/webmain.ecpp \
	$(controllers) \
	$(actions)

notySources :=  $(shell find $(SRC_PUBLIC)/js/noty/ -iname '*.js')

staticSources := \
	$(notySources) \
	$(SRC_PUBLIC)/js/jquery-2.1.4.min.js \
	$(SRC_PUBLIC)/js/tntnet.js \
	$(SRC_PUBLIC)/mypage.js \
	$(SRC_PUBLIC)/MyApi.css

MyApi_SOURCES = \
	src/main.cpp \
	src/configuration.cpp \
	$(ecppSources)

nodist_MyApi_SOURCES := \
	src/public.cpp

MyApi_LDADD = -lcxxtools -ltntnet

nobase_dist_noinst_DATA := $(staticSources)

CLEANFILES := $(ecppSources:.ecpp=.cpp) $(ecppSources:.ecpp=.deps) src/public.cpp

EXTRA_DIST := $(staticSources)

#
# Rules for tntnet apilications
#

ECPPFLAGS=-I$(srcdir)/src

# public
src/public.cpp: $(staticSources) Makefile.am
	$(AM_V_GEN)$(ECPPC) -bb -z -n public -p -o src/public.cpp $(ECPPFLAGS) $(staticSources)

SUFFIXES=.ecpp .cpp

.ecpp.cpp:
	$(AM_V_GEN)$(ECPPC) -n `echo $<|$(SED) 's|^$(srcdir)/||; s|\.ecpp$$||'` $(ECPPFLAGS) -o $@ $<
.ecpp.deps:
	$(AM_V_GEN)$(ECPPC) -M $(ECPPFLAGS) $< | $(SED) '1s|\(.*\).cpp:|\1.cpp $@:|' > $@

-include $(ecppSources:.ecpp=.deps)
