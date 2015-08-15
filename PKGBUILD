# Maintainer: Doug Newgard <scimmia22 at outlook dot com>
# Contributor: Ronald van Haren <ronald.archlinux.org>

pkgname=edje-svn
pkgver=82093
pkgrel=1
pkgdesc="Abstract GUI layout and animation object library in EFL"
arch=('i686' 'x86_64')
url="http://www.enlightenment.org"
license=('BSD' 'GPL2')
depends=('efl-svn' 'lua' 'shared-mime-info')
optdepends=('python2: inkscape2edc')
makedepends=('subversion')
conflicts=('edje')
provides=('edje')
install=edje.install
options=('!libtool')

_svntrunk="http://svn.enlightenment.org/svn/e/trunk/edje"
_svnmod="edje"

build() {
  cd "$srcdir"

  msg "Connecting to SVN server...."

  if [[ -d "$_svnmod/.svn" ]]; then
    (cd "$_svnmod" && svn up -r "$pkgver")
  else
    svn co "$_svntrunk" --config-dir ./ -r "$pkgver" "$_svnmod"
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting build..."

  rm -rf "$srcdir/$_svnmod-build"
  svn export "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build"

  # python2 fix
  sed -i 's_#!/usr/bin/env python_#!/usr/bin/env python2_' utils/inkscape2edc

  ./autogen.sh --prefix=/usr \
	--enable-amalgamation

  make
}

package(){
  cd "$srcdir/$_svnmod-build"

  make DESTDIR="$pkgdir" install

# install license files
  sed -n '/Copyright notice/,/is GPLv2./p' COPYING > COPYING2
  install -Dm644 "$srcdir/$_svnmod-build/COPYING2" \
	"$pkgdir/usr/share/licenses/$pkgname/COPYING"

  install -Dm644 "$srcdir/$_svnmod-build/AUTHORS" \
	"$pkgdir/usr/share/licenses/$pkgname/AUTHORS"

  rm -r "$srcdir/$_svnmod-build"
}
