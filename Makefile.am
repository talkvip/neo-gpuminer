
ACLOCAL_AMFLAGS = -I m4

JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson-2.5/src

if WANT_USBUTILS
USBUTILS_INCLUDES = -I$(top_srcdir)/compat/libusb-1.0/libusb
else
USBUTILS_INCLUDES =
endif

EXTRA_DIST	= example.conf m4/gnulib-cache.m4 linux-usb-cgminer \
		  ADL_SDK/readme.txt api-example.php miner.php	\
		  API.class API.java api-example.c windows-build.txt \
		  bitstreams/* API-README FPGA-README SCRYPT-README \
		  bitforce-firmware-flash.c hexdump.c ASIC-README \
		  01-cgminer.rules GPU-README

SUBDIRS		= lib compat ccan

AM_CPPFLAGS	= $(PTHREAD_FLAGS) -fno-strict-aliasing $(JANSSON_INCLUDES) $(USBUTILS_INCLUDES)

bin_PROGRAMS	= cgminer

bin_SCRIPTS	= $(top_srcdir)/*.cl

cgminer_LDFLAGS	= $(PTHREAD_FLAGS)
cgminer_LDADD	= $(DLOPEN_FLAGS) @LIBCURL_LIBS@ @JANSSON_LIBS@ @PTHREAD_LIBS@ \
		  @OPENCL_LIBS@ @CURSES_LIBS@ @WS2_LIBS@ \
		  @LIBUSB_LIBS@ @MM_LIBS@ @RT_LIBS@ \
		  @MATH_LIBS@ lib/libgnu.a ccan/libccan.a

if HAVE_WINDOWS
cgminer_CPPFLAGS = -I$(top_builddir)/lib -I$(top_srcdir)/lib @OPENCL_FLAGS@
else
cgminer_CPPFLAGS = -I$(top_builddir)/lib -I$(top_srcdir)/lib @OPENCL_FLAGS@ @LIBCURL_CFLAGS@
endif

cgminer_CPPFLAGS += $(ADL_CPPFLAGS)
cgminer_CPPFLAGS += $(NVML_CFLAGS)

# common sources
cgminer_SOURCES := cgminer.c

cgminer_SOURCES	+= elist.h miner.h compat.h bench_block.h	\
		   util.c util.h uthash.h logging.h		\
		   sha2.c sha2.h api.c

cgminer_SOURCES	+= logging.c

# GPU sources, TODO: make them selectable
# the GPU portion extracted from original main.c
cgminer_SOURCES += driver-opencl.h driver-opencl.c

# the original GPU related sources, unchanged
cgminer_SOURCES += ocl.c ocl.h findnonce.c findnonce.h
cgminer_SOURCES += adl.c adl.h adl_functions.h
cgminer_SOURCES += nvml.c nvml.h
#cgminer_SOURCES += *.cl

if HAS_SCRYPT
cgminer_SOURCES += scrypt.c scrypt.h
endif

if HAS_NEOSCRYPT
cgminer_SOURCES += neoscrypt.c neoscrypt.h
endif

if HAS_KECCAK
cgminer_SOURCES += keccak.c keccak.h
endif

if NEED_FPGAUTILS
cgminer_SOURCES += fpgautils.c fpgautils.h
endif

if WANT_USBUTILS
cgminer_SOURCES += usbutils.c usbutils.h
endif

if HAS_BFLSC
cgminer_SOURCES += driver-bflsc.c driver-bflsc.h
endif

if HAS_BITFORCE
cgminer_SOURCES += driver-bitforce.c
endif

if HAS_BITFURY
cgminer_SOURCES += driver-bitfury.c driver-bitfury.h
endif

if HAS_ICARUS
cgminer_SOURCES += driver-icarus.c
endif

if HAS_AVALON
cgminer_SOURCES += driver-avalon.c driver-avalon.h
endif

if HAS_KLONDIKE
cgminer_SOURCES += driver-klondike.c
endif

if HAS_MODMINER
cgminer_SOURCES += driver-modminer.c
bitstreamsdir = $(bindir)/bitstreams
dist_bitstreams_DATA = $(top_srcdir)/bitstreams/*
endif
