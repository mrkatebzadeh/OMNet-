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
	LD_LIBRARY_PATH=${srcdir}/${_pkgname}-${pkgver}/lib:$LD_LIBRARY_PATH
	echo WITH_OSGEARTH=no >> configure.user
    sed -i '/for arg in \$ac_configure_args/,+8 d' configure
	./configure --prefix=/usr WITH_OSGEARTH=no


	make || return 1
}

package() {
	cd ${srcdir}
	mkdir -p "${pkgdir}"/opt
	mv  "${_pkgname}-${pkgver}" ${pkgdir}/opt/${_pkgname} || return 1

	# run OMNeT++ as a normal user
	touch ${pkgdir}/opt/${_pkgname}/ide/error.log
	chmod 777 ${pkgdir}/opt/${_pkgname}/ide/error.log

	# copy profile.d file
	mkdir -p ${pkgdir}/etc/profile.d/

	# copy desktop shortcut
	mkdir -p ${pkgdir}/usr/share/applications/
	cp OMNeT++.desktop ${pkgdir}/usr/share/applications/

}
