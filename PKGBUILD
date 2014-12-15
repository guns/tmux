# Maintainer: Sung Pae <self@sungpae.com>
pkgname=tmux-nerv
pkgver=
pkgrel=1
pkgdesc="Custom tmux build"
arch=('x86_64')
url="https://github.com/guns/tmux"
license=('BSD')
groups=('nerv')
depends=('ncurses' 'libevent' 'libutempter')
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
