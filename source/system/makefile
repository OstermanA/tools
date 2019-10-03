# Initialize env vars
ifeq ($(strip $(PREFIX)),)
PREFIX=$(shell dirname $(shell dirname $(PWD)))/system
endif

SOURCE=$(PWD)

export LDFLAGS=-Wl,-rpath,$(PREFIX)/lib -Wl,-rpath,$(PREFIX)/lib64 -L$(PREFIX)/lib -L$(PREFIX)/lib64
export CFLAGS=-Wl,-rpath,$(PREFIX)/lib -Wl,-rpath,$(PREFIX)/lib64 -I$(PREFIX)/include
export CPPFLAGS=$(CFLAGS)
export CPATH=$(PREFIX)/include
export LD_LIBRARY_PATH=$(PREFIX)/lib:$(PREFIX)/lib64

ifdef $(PROXY)
export http_proxy=$(PROXY)
export https_proxy=$(http_proxy)
export ftp_proxy=$(http_proxy)
export HTTP_PROXY=$(http_proxy)
export HTTPS_PROXY=$(http_proxy)
export FTP_PROXY=$(http_proxy)
endif

# Software Package Verions
autoconf=2.69
automake=1.16.1
gcc=9.2.0
gmp=6.1.2
mpc=1.1.0
mpfr=4.0.2
libtool=2.4.6

# Software File Names
autoconffile=autoconf-$(autoconf).tar.xz
automakefile=automake-$(automake).tar.xz
gccfile=gcc-$(gcc).tar.xz
gmpfile=gmp-$(gmp).tar.xz
mpcfile=mpc-$(mpc).tar.gz
mpfrfile=mpfr-$(mpfr).tar.xz
libtoolfile=libtool-$(libtool).tar.gz

# Software Directory Names
autoconfdir=$(basename $(basename $(autoconffile)))
automakedir=$(basename $(basename $(automakefile)))
gccdir=$(basename $(basename $(gccfile)))
gmpdir=$(basename $(basename $(gmpfile)))
mpcdir=$(basename $(basename $(mpcfile)))
mpfrdir=$(basename $(basename $(mpfrfile)))
libtooldir=$(basename $(basename $(libtoolfile)))

# Software Sources
autoconfsource=http://ftp.gnu.org/gnu/autoconf/$(autoconffile)
automakesource=http://ftp.gnu.org/gnu/automake/$(automakefile)
gccsource=https://ftp.gnu.org/gnu/gcc/gcc-$(gcc)/$(gccfile)
gmpsource=https://gmplib.org/download/gmp/$(gmpfile)
mpcsource=https://ftp.gnu.org/gnu/mpc/$(mpcfile)
mpfrsource=https://www.mpfr.org/mpfr-current/$(mpfrfile)
libtoolsource=http://ftpmirror.gnu.org/libtool/$(libtoolfile)

# Build Cleanup Pointers
cleanautoconf=$(wildcard $(autoconfdir)/.)
cleanautomake=$(wildcard $(automakedir)/.)
cleangcc=$(wildcard $(gccdir)/.)
cleangmp=$(wildcard $(gmpdir)/.)
cleanmpc=$(wildcard $(mpcdir)/.)
cleanmpfr=$(wildcard $(mpfrdir)/.)
cleanlibtool=$(wildcard $(libtooldir)/.)

$(VERBOSE).SILENT:

.PHONY: \
	help \
	autoconf \
	automake \
	gcc \
	gmp \
	mpc \
	mpfr \
	libtool \
	.showenv \
	$(cleanautoconf) \
	$(cleanautomake) \
	$(cleangcc) \
	$(cleangmp) \
	$(cleanmpc) \
	$(cleanmpfr) \
	$(cleanlibtool)

help:
	echo "makefile for system utils and dependencies"
	echo "Will built artifacts and install them to PREFIX=$(PREFIX)"
	echo "If PREFIX is set as an environment variable this makefile will use it"
	echo
	echo "Any source code needed will be retrieved."
	echo "Any target will automatically build and install dependencies."
	echo
	printf "%-9s %-7s %s\n" "TARGET" "VERION" "DEPENDENCIES"
	printf "%-9s %-7s %s\n" "autoconf" "$(autoconf)" "none"
	printf "%-9s %-7s %s\n" "automake" "$(automake)" "none"
	printf "%-9s %-7s %s\n" "gcc" "$(gcc)" "gmp mpfr mpc"
	printf "%-9s %-7s %s\n" "gmp" "$(gmp)" "none"
	printf "%-9s %-7s %s\n" "mpc" "$(mpc)" "mpfr"
	printf "%-9s %-7s %s\n" "mpfr" "$(mpfr)" "none"
	printf "%-9s %-7s %s\n" "libtool" "$(libtool)" "none"
	echo
	echo "Special build targets:"
	echo "all   - Build everything"
	echo "help  - Displays this message"
	echo "clean - Removes temp directories"
	echo
	echo "Set VERBOSE to any non-empty value to see debug messages"

all: autoconf libtool automake gcc

showenv:
	echo "########## Dumping environment vars ##########"
	echo "PATH=$(PATH)"
	echo "LD_LIBRARY_PATH=$(LD_LIBRARY_PATH)"
	echo "PREFIX=$(PREFIX)"
	echo "CFLAGS=$(CFLAGS)"
	echo "CPPFLAGS=$(CPPFLAGS)"

autoconf: showenv
	echo "########## Building autoconf $(autoconf) ##########"
	
	test -f "$(autoconffile)" \
		&& echo "Found $(autoconffile)" \
		|| (echo "Retrieving $(autoconfsource)"; curl -Lso "$(autoconffile)" "$(autoconfsource)")
	tar -xf "$(autoconffile)"
	
	cd "$(autoconfdir)" \
	&& ./configure \
		--prefix="$(PREFIX)" \
	&& make \
	&& make install

automake: showenv
	echo "########## Building automake $(automake) ##########"
	
	test -f "$(automakefile)" \
		&& echo "Found $(automakefile)" \
		|| (echo "Retrieving $(automakesource)"; curl -Lso "$(automakefile)" "$(automakesource)")
	tar -xf "$(automakefile)"
	
	cd "$(automakedir)" \
	&& ./configure \
		--prefix="$(PREFIX)" \
	&& make \
	&& make install

gcc: showenv 
	#gmp mpc mpfr
	echo "########## Building gcc $(gcc) ##########"
	
	test -f "$(gccfile)" \
		&& echo "Found $(gccfile)" \
		|| (echo "Retrieving $(gccsource)"; curl -Lso "$(gccfile)" "$(gccsource)")
	tar -xf "$(gccfile)"
	
	cd "$(gccdir)" \
	&& LD_LIBRARY_PATH="$(LD_LIBRARY_PATH):$(SOURCE)/$(mpcdir)/src/.lib:$(SOURCE)/$(mpfrdir)/src/.lib" ./configure \
		--prefix="$(PREFIX)" \
		--with-gmp="$(PREFIX)" \
		--with-mpfr="$(PREFIX)" \
		--with-mpc="$(PREFIX)" \
	&& make -j12 \
	&& make install

gmp: showenv
	echo "########## Building gmp $(gmp) ##########"
	
	test -f "$(gmpfile)" \
		&& echo "Found $(gmpfile)" \
		|| (echo "Retrieving $(gmpsource)"; curl -Lso "$(gmpfile)" "$(gmpsource)")
	tar -xf "$(gmpfile)"
	
	cd "$(gmpdir)" \
	&& ./configure \
		--prefix="$(PREFIX)" \
	&& make \
	&& make install

libtool: showenv
	echo "########## Building libtool $(libtool) ##########"
	
	test -f "$(libtoolfile)" \
		&& echo "Found $(libtoolfile)" \
		|| (echo "Retrieving $(libtoolsource)"; curl -Lso "$(libtoolfile)" "$(libtoolsource)")
	tar -xf "$(libtoolfile)"
	
	cd "$(libtooldir)" \
	&& ./configure \
		--prefix="$(PREFIX)" \
	&& make \
	&& make install

mpc: showenv mpfr
	echo "########## Building mpc $(mpc) ##########"
	
	test -f "$(mpcfile)" \
		&& echo "Found $(mpcfile)" \
		|| (echo "Retrieving $(mpcsource)"; curl -Lso "$(mpcfile)" "$(mpcsource)")
	tar -xf "$(mpcfile)"
	
	cd "$(mpcdir)" \
	&& ./configure \
		--prefix="$(PREFIX)" \
	&& make \
	&& make install

mpfr: showenv
	echo "########## Building mpfr $(mpfr) ##########"
	
	test -f "$(mpfrfile)" \
		&& echo "Found $(mpfrfile)" \
		|| (echo "Retrieving $(mpfrsource)"; curl -Lso "$(mpfrfile)" "$(mpfrsource)")
	tar -xf "$(mpfrfile)"
	
	cd "$(mpfrdir)" \
	&& ./configure \
		--prefix="$(PREFIX)" \
	&& make \
	&& make install

clean: | \
	$(cleanautoconf) \
	$(cleanautomake) \
	$(cleangcc) \
	$(cleangmp) \
	$(cleanlibtool) \
	$(cleanmpc) \
	$(cleanmpfr)
	echo "########## Done Cleaning ##########"

$(cleanautoconf):
	echo "########## Removing $(autoconfdir) ##########"
	rm -rf "$(autoconfdir)"

$(cleanautomake):
	echo "########## Removing $(automakedir) ##########"
	rm -rf "$(automakedir)"

$(cleangcc):
	echo "########## Removing $(gccdir) ##########"
	rm -rf "$(gccdir)"

$(cleangmp):
	echo "########## Removing $(gmpdir) ##########"
	rm -rf "$(gmpdir)"

$(cleanmpc):
	echo "########## Removing $(mpcdir) ##########"
	rm -rf "$(mpcdir)"

$(cleanmpfr):
	echo "########## Removing $(mpfrdir) ##########"
	rm -rf "$(mpfrdir)"

$(cleanlibtool):
	echo "########## Removing $(libtooldir) ##########"
	rm -rf "$(libtooldir)"

