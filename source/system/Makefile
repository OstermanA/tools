#initialize env vars
ifeq ($(strip $(PREFIX)),)
PREFIX=$(shell dirname $(dir $(PWD)))/$(notdir $(PWD))
endif
SOURCE=$(PWD)

LDFLAGS=-Wl,-rpath,$(PREFIX)/lib -Wl,-rpath,$(PREFIX)/lib64 -L$(PREFIX)/lib -L$(PREFIX)/lib64
CFLAGS=-Wl,-rpath,$(PREFIX)/lib -Wl,-rpath,$(PREFIX)/lib64 -I$(PREFIX)/include
CPPFLAGS=$(CFLAGS)
CXXFLAGS=$(CPPFLAGS)
LD_LIBRARY_PATH=$(PREFIX)/lib:$(PREFIX)/lib64

export LDFLAGS CFLAGS CPPFLAGS CXXFLAGS PREFIX LD_LIBRARY_PATH

export http_proxy=$(PROXY)
export https_proxy=$(PROXY)
export ftp_proxy=$(http_proxy)
export HTTP_PROXY=$(http_proxy)
export HTTPS_PROXY=$(https_proxy)
export FTP_PROXY=$(ftp_proxy)

#Software Package Versions
autoconf=2.69
automake=1.16.1
cmake=3.16.5
gcc=9.2.0
libtool=2.4.6
openssl=1.1.1d

#Derived Global Vars
autoconffile=autoconf-$(autoconf).tar.xz
automakefile=automake-$(automake).tar.xz
cmakefile=cmake-$(cmake).tar.gz
gccfile=gcc-$(gcc).tar.xz
libtoolfile=libtool-$(libtool).tar.gz
opensslfile=openssl-$(openssl).tar.gz

autoconfsource=http://ftp.gnu.org/gnu/autoconf/$(autoconffile)
automakesource=http://ftp.gnu.org/gnu/automake/$(automakefile)
cmakesource=https://github.com/Kitware/CMake/releases/download/v$(cmake)/$(cmakefile)
gccsource=https://ftp.gnu.org/gnu/gcc/gcc-$(gcc)/$(gccfile)
libtoolsource=http://ftpmirror.gnu.org/libtool/$(libtoolfile)
opensslsource=https://www.openssl.org/source/$(opensslfile)

autoconfdir=$(basename $(basename $(autoconffile)))
automakedir=$(basename $(basename $(automakefile)))
cmakedir=$(basename $(basename $(cmakefile)))
gccdir=$(basename $(basename $(gccfile)))
libtooldir=$(basename $(basename $(libtoolfile)))
openssldir=$(basename $(basename $(opensslfile)))

cleanautoconf=$(wildcard $(autoconfdir)/.)
cleanautomake=$(wildcard $(automakedir)/.)
cleancmake=$(wildcard $(cmakedir)/.)
cleangcc=$(wildcard $(gccdir)/.)
cleangccbuild=$(wildcard $(gccdir)_build/.)
cleanlibtool=$(wildcard $(libtooldir)/.)
cleanopenssl=$(wildcard $(openssldir)/.)

ifdef DEPENDS
autoconfdepends=showenv libtool
automakedepends=showenv
cmakedepends=showenv openssl
gccdepends=showenv
libtooldepends=showenv
openssldepends=showenv
else
autoconfdepends=showenv
automakedepends=showenv
cmakedepends=showenv
gccdepends=showenv
libtooldepends=showenv
openssldepends=showenv
endif

.PHONY: \
	help \
	showenv \
	autoconf \
	automake \
	cmake \
	gcc \
	libtool \
	openssl \
	$(cleanautoconf) \
	$(cleanautomake) \
	$(cleancmake) \
	$(cleangcc) \
	$(cleangccbuild) \
	$(cleanlibtool)	\
	$(cleanopenssl)



help:
	echo "Help stuff goes here later"

all: gcc autoconf automake cmake libtool

$(VERBOSE).SILENT:

showenv:
	echo "INFO: Dumping build vars:"
	echo "INFO: PREFIX          = $(PREFIX)"
	echo "INFO: LDFLAGS         = $(LDFLAGS)"
	echo "INFO: CFLAGS          = $(CFLAGS)"
	echo "INFO: CPPFLAGS        = $(CPPFLAGS)"
	echo "INFO: CXXFLAGS        = $(CXXFLAGS)"
	echo "INFO: LD_LIBRARY_PATH = $(LD_LIBRARY_PATH)"

autoconf: $(autoconfdepends)
	echo "########################################"
	echo "INFO: Building autoconf $(autoconf)"
	
	test -f "$(autoconffile)" \
	&& echo "INFO: Found $(autoconffile)" \
	|| (echo "INFO: Retrieving $(autoconfsource)"; curl -Lso "$(autoconffile)" "$(autoconfsource)")
	
	echo "INFO: Unpacking $(autoconffile)"
	tar -xf "$(autoconffile)"
	
	echo "INFO: Running $(autoconfdir)/configure"
	cd "$(autoconfdir)" && ./configure \
		--prefix="$(PREFIX)"
	
	echo "INFO: Running 'make'"
	cd "$(autoconfdir)" && make -j12
	
	if [[ -n $$CHECK ]]; then \
		echo "INFO: Running 'make check'"; \
		cd "$(autoconfdir)" && make check; \
	else echo "INFO: Skipping 'make check'"; fi
	
	echo "INFO: Running 'make install'"
	cd "$(autoconfdir)" && make install
	
	echo "INFO: Successfully built autoconf $(autoconf)"

automake: $(automakedepends)
	echo "########################################"
	echo "INFO: Building automake $(automake)"
	
	test -f "$(automakefile)" \
	&& echo "INFO: Found $(automakefile)" \
	|| (echo "INFO: Retrieving $(automakesource)"; curl -Lso "$(automakefile)" "$(automakesource)")
	echo "INFO: Unpacking $(automakefile)"
	tar -xf "$(automakefile)"
	
	echo "INFO: Running $(automakedir)/configure"
	cd "$(automakedir)" && ./configure \
		--prefix="$(prefix)"
	
	echo "INFO: Running 'make'"
	cd "$(automakedir)" && make -j12
	
	if [[ -n $$CHECK ]]; then \
		echo "INFO: Running 'make check'"; \
		cd "$(automakedir)" && make check; \
	else echo "INFO: Skipping 'make check'"; fi
	
	echo "INFO: Running 'make install'"
	cd "$(automakedir)" && make install
	
	echo "INFO: Successfully built automake $(automake)"

cmake: $(cmakedepends)
	echo "########################################"
	echo "INFO: Building cmake $(cmake)"
	
	test -f "$(cmakefile)" \
	&& echo "INFO: Found $(cmakefile)" \
	|| (echo "INFO: Retrieving $(cmakesource)"; curl -Lso "$(cmakefile)" "$(cmakesource)")
	echo "INFO: Unpacking $(cmakefile)"
	tar -xf "$(cmakefile)"
	
	echo "INFO: Running $(cmakedir)/configure"
	cd "$(cmakedir)" && ./bootstrap \
		--prefix="$(PREFIX)"
	
	echo "INFO: Running 'make'"
	cd "$(cmakedir)" && make -j12
	
	if [[ -n $$CHECK ]]; then \
		echo "INFO: Running 'make test'"; \
		cd "$(cmakedir)" && make test; \
	else echo "INFO: Skipping 'make check'"; fi
	
	echo "INFO: Running 'make install'"
	cd "$(cmakedir)" && make install
	
	echo "INFO: Successfully built cmake $(cmake)"

gcc: $(gccdepends)
	echo "########################################"
	echo "INFO: Building gcc $(gcc)"
	
	test -f "$(gccfile)" \
	&& echo "INFO: Found $(gccfile)" \
	|| (echo "INFO: Retrieving $(gccsource)"; curl -Lso "$(gccfile)" "$(gccsource)")
	echo "INFO: Unpacking $(gccfile)"
	tar -xf "$(gccfile)"
	
	mkdir -p "$(gccdir)_build"
	
	echo "INFO: Download prerequisites"
	cd "$(gccdir)" && ./contrib/download_prerequisites
	
	echo "INFO: Running $(gccdir)/configure"
	cd "$(gccdir)_build" && "$(SOURCE)/$(gccdir)/configure" \
		--prefix="$(PREFIX)" \
		--with-boot-ldflags="-static-libstdc++ -static-libgcc $(LDFLAGS)"
	
	echo "INFO: Running 'make'"
	cd "$(gccdir)_build" && make -j12 \
	
	if [[ -n $$CHECK ]]; then
		echo "INFO: Running 'make check'"; \
		make check; \
	else echo "INFO: Skipping 'make check'"; fi
	
	echo "INFO: Running 'make install'"
	cd "$(gccdir)_build" && make install
	
	echo "INFO: Successfully built gcc $(gcc)"

libtool: $(libtooldepends)
	echo "########################################"
	echo "INFO: Building libtool $(libtool)"
	
	test -f "$(libtoolfile)" \
	&& echo "INFO: Found $(libtoolfile)" \
	|| (echo "INFO: Retrieving $(libtoolsource)"; curl -Lso "$(libtoolfile)" "$(libtoolsource)")
	echo "INFO: Unpacking $(libtoolfile)"
	tar -xf "$(libtoolfile)"
	
	echo "INFO: Running $(libtooldir)/configure"
	cd "$(libtooldir)" && ./configure \
		--prefix="$(PREFIX)"
	
	echo "INFO: Running 'make'"
	cd "$(libtooldir)" && make
	
	if [[ -n $$CHECK ]]; then
		echo "INFO: Running 'make check'"; \
		cd "$(libtooldir)" && make check; \
	else echo "INFO: Skipping 'make check'"; fi
	
	echo "INFO: Running 'make install'"
	cd "$(libtooldir)" && make install
	
	echo "INFO: Successfully built libtool $(libtool)"

openssl: $(openssldepends)
	echo "########################################"
	echo "INFO: Building openssl $(openssl)"
	
	test -f "$(opensslfile)" \
	&& echo "INFO: Found $(opensslfile)" \
	|| (echo "INFO: Retrieving $(opensslsource)"; curl -Lso "$(opensslfile)" "$(opensslsource)")
	echo "INFO: Unpacking $(opensslfile)"
	tar -xf "$(opensslfile)"
	
	echo "INFO: Running $(openssldir)/configure"
	cd "$(openssldir)" && ./config \
		--prefix="$(PREFIX)" \
		--openssldir="$(PREFIX)" \
		threads \
		shared \
		no-comp \
		"$(CFLAGS)"
	
	echo "INFO: Running 'make depend'"
	cd "$(openssldir)" && make depend
	
	echo "INFO: Running 'make'"
	cd "$(openssldir)" && make -j12
	
	if [[ -n $$CHECK ]]; then \
		echo "INFO: Running 'make test'"; \
		cd "$(openssldir)" && make test; \
	else echo "INFO: Skipping 'make check'"; fi
	
	echo "INFO: Running 'make install_sw install_ssldirs'"
	cd "$(openssldir)" && make install_sw install_ssldirs

clean: |\
	$(cleanautoconf) \
	$(cleanautomake) \
	$(cleancmake) \
	$(cleangcc) \
	$(cleangccbuild) \
	$(cleanlibtool) \
	$(cleanopenssl)

$(cleanautoconf):
	@echo "INFO: Removing $(autoconfdir)"
	rm -rf "$(autoconfdir)"

$(cleanautomake):
	@echo "INFO: Removing $(automakedir)"
	rm -rf "$(automakedir)"

$(cleancmake):
	@echo "INFO: Removing $(cmakedir)"
	rm -rf "$(cmakedir)"

$(cleangcc):
	@echo "INFO: Removing $(gccdir)"
	rm -rf "$(gccdir)"

$(cleangccbuild):
	@echo "INFO: Removing $(gccdir)_build"
	rm -rf "$(gccdir)_build"

$(cleanlibtool):
	@echo "INFO: Removing $(libtooldir)"
	rm -rf "$(libtooldir)"

$(cleanopenssl):
	@echo "INFO: Removing $(openssldir)"
	rm -rf "$(libtooldir)"
