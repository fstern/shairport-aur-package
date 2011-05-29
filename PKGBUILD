# See http://wiki.archlinux.org/index.php/VCS_PKGBUILD_Guidelines
# for more information on packaging from GIT sources.

# Maintainer: Falk Stern <falk@fourecks.de>
pkgname=shairport-git
pkgver=20110528
pkgrel=2
pkgdesc="an AirTunes/raop server"
arch=(i686 x86_64)
url="https://github.com/fstern/shairport"
license=('shairport')
depends=(avahi libao openssl perl-libwww perl-crypt-openssl-rsa)
optdepends=('perl-io-socket-inet6: to work with iTunes')
makedepends=('git' libao openssl)
provides=(shairport)
source=('shairport.confd'
	'shairport.init')
md5sums=('f53311177bffbac70611d7e33bebe827'
         'c3145311b03ca709700e0846e7f8452d')

_gitroot="https://github.com/fstern/shairport.git"
_gitname="shairport"

build() {
  cd "$srcdir"
  msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  cd "$srcdir/$_gitname-build"

  #
  # BUILD HERE
  #

  make
}

package() {
  install -Dm644 ../shairport.confd "${pkgdir}"/etc/conf.d/shairport
  install -Dm755 ../shairport.init "${pkgdir}"/etc/rc.d/shairport
  cd "$srcdir/$_gitname-build"
  make DESTDIR="$pkgdir/" prefix=/usr install
  install -Dm644 LICENSE "${pkgdir}"/usr/share/licenses/${pkgname}/LICENSE
  install -Dm644 README.md "${pkgdir}"/usr/share/doc/${pkgname}/README.md
  
} 
