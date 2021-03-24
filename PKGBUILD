# Maintainer: M.R. Siavash Katebzadeh <mr.katebzadeh@gmail.com>
pkgname="omnetpp"
pkgver=5.5.1
pkgrel=1
epoch=
pkgdesc="Component-based simulation package designed for modeling communication networks"
arch=('i686' 'x86_64')
url="http://www.omnetpp.org"
license=("Academic Public License")
depends=('tcl' 'tk' 'qt5-base')
makedepends=('libxml2' 'bison' 'flex')
install=${pkgname}.install
_pkgname="omnetpp"

DLAGENTS=(
  "http::/usr/bin/wget --no-check-certificate -c -r -np -nd -H --referer https://omnetpp.org/ %u"
  "https::/usr/bin/wget --no-check-certificate -c -r -np -nd -H --referer https://omnetpp.org/ %u"
)
source=(
    omnetpp-${pkgver}-src-linux.tgz::"https://github.com/omnetpp/omnetpp/releases/download/omnetpp-${pkgver}/omnetpp-${pkgver}-src-linux.tgz"
    OMNeT++.desktop
    )
md5sums=('f7abe260ff47ec02a665e287c653db86'
         '4e51f984f1a7114ab1f0b6f88fa4e0bc')

build() {
	cd ${srcdir}/${_pkgname}-${pkgver}
	PATH=${srcdir}/${_pkgname}-${pkgver}/bin:$PATH
    changeText1=". .\/configure.user"
    changeText2=".\/configure.user"
	LD_LIBRARY_PATH=${srcdir}/${_pkgname}-${pkgver}/lib:$LD_LIBRARY_PATH

	sed -i 's!OMNETPP_ROOT/images!OMNETPP_ROOT/images;/usr/share/omnetpp/images!' configure*
    sed -i '/for arg in \$ac_configure_args/,+8 d' configure
    sed -i 's/WITH_OSGEARTH/WITH_OSGEARTH=no/g' configure
	WITH_OSGEARTH=no ./configure --prefix=/usr

	sed -i 's!IDEDIR=.*!IDEDIR=/opt/omnetpp/ide!' src/utils/omnetpp src/utils/omnest
	sed -i 's!osgi.instance.area.default=.*$!#osgi.instance.area.default=/usr/share/omnetpp/samples!g' ide/configuration/config.ini

	make || return 1
}

package() {
	cd ${srcdir}/${_pkgname}-${pkgver}
	mkdir -p ${pkgdir}/usr/bin
	install -m755 bin/* ${pkgdir}/usr/bin

	sed "s|${srcdir}/${_pkgname}-${pkgver}|/usr|g" -i ${
    kgdir}/usr/bin/opp_makemake
	sed "s|OMNETPP_INCL_DIR=/usr/include|OMNETPP_INCL_DIR=/usr/include/omnetpp|" -i ${pkgdir}/usr/bin/opp_makemake

	mkdir -p ${pkgdir}/usr/lib
	mkdir -p ${pkgdir}/opt
	install lib/* ${pkgdir}/usr/lib

	mkdir -p ${pkgdir}/usr/include/omnetpp
	mkdir -p ${pkgdir}/usr/include/omnetpp/platdep
	install -m644 include/*.h ${pkgdir}/usr/include/omnetpp
	install -m644 include/platdep/*.h ${pkgdir}/usr/include/omnetpp/platdep

	install -d ${pkgdir}/usr/share/omnetpp/{images,doc,samples}
	cp -R images/* ${pkgdir}/usr/share/omnetpp/images
	cp -R doc/* ${pkgdir}/usr/share/omnetpp/doc
	cp -R samples/* ${pkgdir}/opt/omnetpp

	install -d ${pkgdir}/usr/share/emacs/site-lisp
	install -m644 misc/emacs/ned-mode.el ${pkgdir}/usr/share/emacs/site-lisp
	install -m644 misc/emacs/ini-mode.el ${pkgdir}/usr/share/emacs/site-lisp

	install -d ${pkgdir}/opt/omnetpp
	cp -R ide ${pkgdir}/opt/omnetpp
	touch ${pkgdir}/opt/omnetpp/ide/error.log
	chmod a+rw ${pkgdir}/opt/omnetpp/ide/error.log

	mkdir -p ${pkgdir}/usr/share/icons/
	cp ${srcdir}/omnetpp-${pkgver}/ide/icon.png ${pkgdir}/usr/share/icons/omnetpp.png

	mkdir -p ${pkgdir}/usr/share/applications/
	cd ${srcdir}
	cp OMNeT++.desktop ${pkgdir}/usr/share/applications/
}
