MAINTAINERCLEANFILES = $(srcdir)/Makefile.in $(srcdir)/config.h.in
CLEANFILES = dnsperf-workbench.1 resperf.1 *.gcda *.gcno *.gcov \
  */*.gcda */*.gcno */*.gcov

SUBDIRS = test

AM_CFLAGS = -I$(srcdir) \
  -I$(top_srcdir) \
  $(PTHREAD_CFLAGS) $(libssl_CFLAGS) $(libcrypto_CFLAGS) $(libldns_CFLAGS)

EXTRA_DIST = dnsperf-workbench.1.in resperf-report resperf.1.in

bin_PROGRAMS = dnsperf-workbench resperf
dist_bin_SCRIPTS = resperf-report

_libperf_sources = datafile.c dns.c log.c net.c opt.c os.c strerror.c qtype.c \
  edns.c tsig.c net_udp.c net_tcp.c net_dot.c net_doh.c ext/parse_uri.c

_libperf_headers = datafile.h dns.h log.h net.h opt.h os.h util.h strerror.h \
  list.h result.h buffer.h qtype.h edns.h tsig.h ext/parse_uri.h

if HAVE_STDATOMIC
_libperf_sources += ext/hg64.c
_libperf_headers += ext/hg64.h
endif

dnsperf_workbench_SOURCES = $(_libperf_sources) dnsperf.c
dist_dnsperf_workbench_SOURCES = $(_libperf_headers)
dnsperf_workbench_LDADD = $(PTHREAD_LIBS) $(libssl_LIBS) $(libcrypto_LIBS) \
  $(libldns_LIBS) $(libnghttp2_LIBS)

resperf_SOURCES = $(_libperf_sources) resperf.c
dist_resperf_SOURCES = $(_libperf_headers)
resperf_LDADD = $(PTHREAD_LIBS) $(libssl_LIBS) $(libcrypto_LIBS) \
  $(libldns_LIBS) $(libnghttp2_LIBS)

man1_MANS = dnsperf-workbench.1 resperf.1

dnsperf-workbench.1: dnsperf-workbench.1.in Makefile
	sed -e 's,[@]PACKAGE_VERSION[@],$(PACKAGE_VERSION),g' \
	-e 's,[@]PACKAGE_URL[@],$(PACKAGE_URL),g' \
	-e 's,[@]PACKAGE_BUGREPORT[@],$(PACKAGE_BUGREPORT),g' \
	< $(srcdir)/dnsperf-workbench.1.in > dnsperf-workbench.1

resperf.1: resperf.1.in Makefile
	sed -e 's,[@]PACKAGE_VERSION[@],$(PACKAGE_VERSION),g' \
	-e 's,[@]PACKAGE_URL[@],$(PACKAGE_URL),g' \
	-e 's,[@]PACKAGE_BUGREPORT[@],$(PACKAGE_BUGREPORT),g' \
	< $(srcdir)/resperf.1.in > resperf.1

if ENABLE_GCOV
gcov-local:
	for src in $(_libperf_sources) dnsperf.c resperf.c; do \
	  gcov -l -r -s "$(srcdir)" "$$src"; \
	done
endif
