
SUBDIRS = . tests

AM_CPPFLAGS = -I$(top_srcdir)/src/lib -I$(top_builddir)/src/lib
AM_CPPFLAGS += -I$(top_srcdir)/src/bin -I$(top_builddir)/src/bin
AM_CPPFLAGS += -I$(top_srcdir)/src -I$(top_builddir)/src
AM_CPPFLAGS += $(BOOST_INCLUDES) $(SYSREPO_INCLUDES)

AM_CXXFLAGS = $(KEA_CXXFLAGS)

if USE_STATIC_LINK
AM_LDFLAGS = -static
endif

CLEANFILES  = *.gcno *.gcda netconf_messages.h netconf_messages.cc s-messages

man_MANS = kea-netconf.8
DISTCLEANFILES = $(man_MANS)
EXTRA_DIST = $(man_MANS) kea-netconf.xml
#EXTRA_DIST += netconf.dox netconf_hooks.dox netconfo6.dox
#EXTRA_DIST += netconf_parser.yy

if GENERATE_DOCS
kea-netconf.8: kea-netconf.xml
	@XSLTPROC@ --novalid --xinclude --nonet -o $@ \
        http://docbook.sourceforge.net/release/xsl/current/manpages/docbook.xsl \
	$(srcdir)/kea-netconf.xml

else

$(man_MANS):
	@echo Man generation disabled.  Creating dummy $@.  Configure with --enable-generate-docs to enable it.
	@echo Man generation disabled.  Remove this file, configure with --enable-generate-docs, and rebuild Kea > $@

endif

netconf_messages.h netconf_messages.cc: s-messages

s-messages: netconf_messages.mes
	$(top_builddir)/src/lib/log/compiler/kea-msg-compiler $(top_srcdir)/src/bin/netconf/netconf_messages.mes
	touch $@

BUILT_SOURCES = netconf_messages.h netconf_messages.cc

# convenience archive

noinst_LTLIBRARIES = libnetconf.la

libnetconf_la_SOURCES  = netconf_log.cc netconf_log.h

nodist_libnetconf_la_SOURCES = netconf_messages.h netconf_messages.cc

EXTRA_DIST += netconf_messages.mes

sbin_PROGRAMS = kea-netconf

kea_netconf_SOURCES  = main.cc

kea_netconf_LDADD  = libnetconf.la
kea_netconf_LDADD += $(top_builddir)/src/lib/dhcpsrv/libkea-dhcpsrv.la
kea_netconf_LDADD += $(top_builddir)/src/lib/log/libkea-log.la
kea_netconf_LDADD += $(top_builddir)/src/lib/exceptions/libkea-exceptions.la
kea_netconf_LDADD += $(LOG4CPLUS_LIBS) $(CRYPTO_LIBS) $(BOOST_LIBS) $(SYSREPO_LIBS)


kea_netconfdir = $(pkgdatadir)

if GENERATE_PARSER

#parser: netconf_lexer.cc location.hh position.hh stack.hh netconf_parser.cc netconf_parser.h
#	@echo "Flex/bison files regenerated"

# --- Flex/Bison stuff below --------------------------------------------------
# When debugging grammar issues, it's useful to add -v to bison parameters.
# bison will generate parser.output file that explains the whole grammar.
# It can be used to manually follow what's going on in the parser.
# This is especially useful if yydebug_ is set to 1 as that variable
# will cause parser to print out its internal state.
# Call flex with -s to check that the default rule can be suppressed
# Call bison with -W to get warnings like unmarked empty rules
# Note C++11 deprecated register still used by flex < 2.6.0
#location.hh position.hh stack.hh netconf_parser.cc netconf_parser.h: netconf_parser.yy
#	$(YACC) --defines=netconf_parser.h --report=all --report-file=netconf_parser.report -o netconf_parser.cc netconf_parser.yy

#netconf_lexer.cc: netconf_lexer.ll
#	$(LEX) --prefix netconf_ -o netconf_lexer.cc netconf_lexer.ll

else

parser location.hh position.hh stack.hh netconf_parser.cc netconf_parser.h netconf_lexer.cc:
	@echo Parser generation disabled. Configure with --enable-generate-parser to enable it.

endif
