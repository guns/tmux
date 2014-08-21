# Maintainer: Sung Pae <self@sungpae.com>
pkgname=tmux-guns
pkgver=
pkgrel=1
pkgdesc="Sung Pae's tmux build"
arch=('x86_64')
url="https://github.com/guns/tmux"
license=('BSD')
groups=('guns')
depends=('ncurses' 'libevent')
makedepends=('git' 'ruby')
provides=('tmux')
conflicts=('tmux')

pkgver() {
    printf %s "$(git describe --long | tr - .)"
}

build() {
    cd "$startdir"
    PREFIX=/usr rake configure
    make -j $(grep -c ^processor /proc/cpuinfo)
}

package() {
    cd "$startdir"
    make DESTDIR="$pkgdir/" install
}
