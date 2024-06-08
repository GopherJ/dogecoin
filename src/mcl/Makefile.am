ACLOCAL_AMFLAGS = -I build-aux/m4

AM_CXXFLAGS = -std=c++03 -fpic -g3 -I$(top_srcdir)/include/mcl -I$(top_srcdir)/src $(CFLAGS)

AM_CFLAGS = -std=c++03 -fpic -g3 -I$(top_srcdir)/include/mcl -I$(top_srcdir)/src $(CFLAGS)

libdir = $(prefix)/lib

lib_LTLIBRARIES = libmcl.la

include_HEADERS = include/mcl/bint_proto.hpp src/bint_switch.hpp src/llvm_proto.hpp include/mcl/bn.h include/mcl/impl/bn_c_impl.hpp

libmcl_la_SOURCES = src/fp.cpp src/bn_c384_256.cpp
libmcl_la_CPPFLAGS = -std=c++03 -fpic -g3 -I$(top_srcdir)/include/mcl -I$(top_srcdir)/src $(CFLAGS)
libmcl_la_LDFLAGS = -version-info 1:74:0 @LDFLAGS@

noinst_DATA = src/bint$(BIT).lo src/base$(BIT).lo
libmcl_la_LIBADD = $(noinst_DATA)

mclincludedir = $(includedir)/mcl
mclinclude_HEADERS = $(mcl_headers)

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libmcl.pc

include/mcl/bint_proto.hpp: src/gen_bint_header.py
	python3 $< > $@ proto -max_bit $(MCL_MAX_BIT_SIZE)
src/bint_switch.hpp: src/gen_bint_header.py
	python3 $< > $@ switch -max_bit $(MCL_MAX_BIT_SIZE)
src/llvm_proto.hpp: src/gen_llvm_proto.py
	python3 $< > $@

src/bint64.lo: src/asm/bint-x64-amd64.S
		$(LIBTOOL) --mode=compile $(CC) -c $< -o $@ $(CFLAGS)

src/base64.lo: src/asm/x86-64.S
		$(LIBTOOL) --mode=compile $(CC) -c $< -o $@ $(CFLAGS)

