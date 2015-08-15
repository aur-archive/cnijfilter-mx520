# This package is heavily based on cnijfilter-mg6100
#
# If you're going to adapt this package to other printer models,
# here is a list of printer names and IDs:
#   name - id
# -------------
# mx720 - 416
# mx920 - 417
# mx390 - 418
# mx450 - 419
# mx520 - 420
# e610  - 421
#
# Just change the following variables accordingly:
_name=mx520
_id=420

pkgname=cnijfilter-${_name}
pkgver=3.90
pkgrel=3
_pkgver=3.90-1
pkgdesc="Canon IJ Printer Driver (${_name} series)"
url="http://support-sg.canon-asia.com/contents/SG/EN/0100517102.html"
arch=('i686' 'x86_64')
license=('custom')
depends=('popt' 'gtk2' 'cups')
source=(http://gdlp01.c-wss.com/gds/1/0100005171/01/cnijfilter-source-${_pkgver}.tar.gz
        fix_cups.patch
        fix_configures.patch)
md5sums=('f1b34f42c8de1abd19b85de5f0e80ca0'
         '0610d664c3734a8bec874e2b474c4dac'
         '136b1690d7f1f71c4257eda52e8fb1b3')

if [ "$CARCH" == "x86_64" ]; then  
	_libdir=libs_bin64
else
	_libdir=libs_bin32
fi

build() {
	## Apply patches
	cd ${srcdir}/cnijfilter-source-${_pkgver}
	patch -p1 -i ${srcdir}/fix_cups.patch || return 1
	patch -p1 -i ${srcdir}/fix_configures.patch || return 1

	## Compile model specific stuff
	# ppd file
	msg "Compile PPD file..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/ppd
	./autogen.sh --prefix=/usr --enable-ppdpath=/usr/share/cups/model --program-suffix=${_name}
	make clean || return 1
	make || return 1
	# cnijfilter
	msg "Compile cnijfilter..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cnijfilter
	./autogen.sh --prefix=/usr --enable-libpath=/usr/lib/bjlib --enable-binpath=/usr/bin --program-suffix=${_name}
	make clean || return 1
	make || return 1
	# lgmon
	msg "Compile lgmon..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/lgmon
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin --program-suffix=${_name}
	make clean || return 1
	make || return 1
	# cngpijmon
	msg "Compile cngpijmon..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpijmon
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin --datadir=/usr/share --program-suffix=${_name}
	make clean || return 1
	make || return 1
	# maintenance
	msg "Compile maintenance..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/maintenance
	./autogen.sh --prefix=/usr --datadir=/usr/share --enable-libpath=/usr/lib/bjlib --program-suffix=${_name}
	make clean || return 1
	make || return 1

	## Compile common stuff
	# libs
	msg "Compile libs..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/libs
	./autogen.sh --prefix=/usr
	make clean || return 1
	make || return 1
	# cngpij
	msg "Compile cngpij..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpij
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin
	make clean || return 1
	make || return 1
	# cngpijmnt
	msg "Compile cngpijmnt..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpijmnt
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin
	make clean || return 1
	make || return 1
	# pstocanonij
	msg "Compile pstocanonij..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/pstocanonij
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin
	make clean || return 1
	make || return 1
	# backend
	msg "Compile backend..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/backend
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin
	make clean || return 1
	make || return 1
	# backendnet
	msg "Compile backendnet..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/backendnet
	./autogen.sh --prefix=/usr --enable-progpath=/usr/bin LDFLAGS="-L../../com/${_libdir}"
	make clean || return 1
	make || return 1
	# sm sub process
	msg "Compile sm sub process..."
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpijmon/cnijnpr
	./autogen.sh --prefix=/usr LIBS=-ldl
	make clean || return 1
	make || return 1
}

package() {
	## Install model specific stuff
	# ppd file
	cd ${srcdir}/cnijfilter-source-${_pkgver}/ppd
	make install DESTDIR=${pkgdir} || return 1
	# cnijfilter
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cnijfilter
	make install DESTDIR=${pkgdir} || return 1
	# lgmon
	cd ${srcdir}/cnijfilter-source-${_pkgver}/lgmon
	make install DESTDIR=${pkgdir} || return 1
	# cngpijmon
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpijmon
	make install DESTDIR=${pkgdir} || return 1
	# maintenance
	cd ${srcdir}/cnijfilter-source-${_pkgver}/maintenance
	make install DESTDIR=${pkgdir} || return 1

	## Install common stuff
	# libs
	cd ${srcdir}/cnijfilter-source-${_pkgver}/libs
	make install DESTDIR=${pkgdir} || return 1
	# cngpij
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpij
	make install DESTDIR=${pkgdir} || return 1
	# cngpijmnt
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpijmnt
	make install DESTDIR=${pkgdir} || return 1
	# pstocanonij
	cd ${srcdir}/cnijfilter-source-${_pkgver}/pstocanonij
	make install DESTDIR=${pkgdir} || return 1
	# backend
	cd ${srcdir}/cnijfilter-source-${_pkgver}/backend
	make install DESTDIR=${pkgdir} || return 1
	# backendnet
	cd ${srcdir}/cnijfilter-source-${_pkgver}/backendnet
	make install DESTDIR=${pkgdir} || return 1
	# sm sub process
	cd ${srcdir}/cnijfilter-source-${_pkgver}/cngpijmon/cnijnpr
	make install DESTDIR=${pkgdir} || return 1

	## Install model specific libraries
	install -d ${pkgdir}/usr/lib/
	install -d ${pkgdir}/usr/lib/bjlib/
	cp -d ${srcdir}/cnijfilter-source-${_pkgver}/${_id}/${_libdir}/* ${pkgdir}/usr/lib/
	cp -d ${srcdir}/cnijfilter-source-${_pkgver}/${_id}/database/* ${pkgdir}/usr/lib/bjlib/
		
	## Install common libraries
	cp -d ${srcdir}/cnijfilter-source-${_pkgver}/com/${_libdir}/* ${pkgdir}/usr/lib/
	install -m 666 ${srcdir}/cnijfilter-source-${_pkgver}/com/ini/cnnet.ini ${pkgdir}/usr/lib/bjlib/

	## Install license files
	cd ${srcdir}/cnijfilter-source-${_pkgver}
	install -d ${pkgdir}/usr/share/licenses/${pkgname}/
	install -m 644 LICENSE-* ${pkgdir}/usr/share/licenses/${pkgname}/
}

# vim:set ts=2 sw=2 et:
