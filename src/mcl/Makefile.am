ACLOCAL_AMFLAGS = -I build-aux/m4

AM_CXXFLAGS = -std=c++03 -fpic -g3 @CFLAGS@
AM_CPPFLAGS = -std=c++03 -fpic -g3 @CFLAGS@

AM_CFLAGS = --fpic -g3 @CFLAGS@

lib_LTLIBRARIES = libmcl.la

include_HEADERS = include/mcl/bint_proto.hpp src/bint_switch.hpp src/llvm_proto.hpp include/mcl/bn.h include/mcl/impl/bn_c_impl.hpp include/mcl/bn_c384_256.h

libmcl_la_SOURCES = src/fp.cpp src/bn_c384_256.cpp
libmcl_la_CPPFLAGS = @CFLAGS@
libmcl_la_LDFLAGS = -version-info 1:74:0 @LDFLAGS@

noinst_DATA = src/bint$(BIT).lo src/base$(BIT).lo src/msm_avx.lo
libmcl_la_LIBADD = $(noinst_DATA)

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = libmcl.pc

include/mcl/bint_proto.hpp: src/gen_bint_header.py
	python3 $< > $@ proto -max_bit $(MCL_MAX_BIT_SIZE)
src/bint_switch.hpp: src/gen_bint_header.py
	python3 $< > $@ switch -max_bit $(MCL_MAX_BIT_SIZE)
src/llvm_proto.hpp: src/gen_llvm_proto.py
	python3 $< > $@

src/msm_avx.lo: src/msm_avx.cpp
		$(LIBTOOL) --mode=compile $(CC) -c $< -o $@ @CFLAGS@ -mavx512f -mavx512ifma -std=c++11 -fno-exceptions -fno-rtti -DCYBOZU_DONT_USE_STRING -DCYBOZU_DONT_USE_EXCEPTION -DNDEBUG

src/bint64.lo: src/asm/bint-x64-amd64.S
		$(LIBTOOL) --mode=compile $(CC) -c $< -o $@ @CFLAGS@

src/base64.lo: src/asm/x86-64.S
		$(LIBTOOL) --mode=compile $(CC) -c $< -o $@ @CFLAGS@

