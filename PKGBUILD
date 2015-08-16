# Contributor: demonicmaniac <stormtrooperofdeath@gmx.net>
# Maintainer: Erico Nunes <nunes.erico[at]gmail>

pkgname=mm3d-svn
pkgver=440
pkgrel=4
pkgdesc="Misfit Model 3D is an easy to use OpenGL 3D model editor."
arch=('i686' 'x86_64')
url="http://www.misfitcode.com/misfitmodel3d/"
license=('GPL')
depends=('libgl' 'qt4' 'perl-template-toolkit' 'perl-html-template')
makedepends=('subversion')

_svntrunk=http://svn.misfitcode.com/misfitcode/mm3d/trunk
_svnmod=mm3d

build() {
	cd "$srcdir"

	if [ -d $_svnmod/.svn ]; then
		(cd $_svnmod && svn up -r $pkgver)
	else
		svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
	fi

	msg "SVN checkout done or server timeout"
	msg "Starting make..."

	rm -rf "$srcdir/$_svnmod-build"
	cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
	cd "$srcdir/$_svnmod-build"

	# These are needed to fix some compilation errors from upstream.
	sed -i '58s/push_back( val );/this->push_back(val);/' $srcdir/mm3d-build/src/libmm3d/sorted_list.h
	sed -i '131s/push_back( val );/this->push_back(val);/' $srcdir/mm3d-build/src/libmm3d/sorted_list.h
	sed -i '23 a\#include <unistd.h>/' $srcdir/mm3d-build/src/libmm3d/misc.cc

	./autosetup.sh
	./configure --prefix=/usr

	make
}

package() {
	cd "$srcdir/$_svnmod-build"

	make DESTDIR="$pkgdir" install
}

