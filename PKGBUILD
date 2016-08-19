pkgname=nodejs-lts
pkgver=4.5.0
pkgrel=1
pkgdesc='Evented I/O for V8 javascript (LTS release: Argon)'
arch=('i686' 'x86_64')
url='https://nodejs.org/'
license=('MIT')
depends=('openssl' 'zlib' 'icu') # 'libuv' 'v8' 'c-ares')
makedepends=('python2' 'procps-ng')
optdepends=('npm: nodejs package manager')
provides=('nodejs')
conflicts=('nodejs')
source=("https://github.com/nodejs/node/archive/v$pkgver.tar.gz")
sha256sums=('eef72c6e48b0911fd79738ee36266009e47ddb4cff06227e18a6f004771f3cd1')

prepare() {
  cd node-$pkgver

  msg 'Fixing for python2 name'
  find -type f -exec sed \
    -e 's_^#!/usr/bin/env python$_&2_' \
    -e 's_^\(#!/usr/bin/python2\).[45]$_\1_' \
    -e 's_^#!/usr/bin/python$_&2_' \
    -e 's_^\( *exec \+\)python\( \+.*\)$_\1python2\2_'\
    -e 's_^\(.*\)python\( \+-c \+.*\)$_\1python2\2_'\
    -e "s_'python'_'python2'_" -i {} \;
  find test/ -type f -exec sed 's_python _python2 _' -i {} \;
}

build() {
  cd node-$pkgver

  export PYTHON=python2
  ./configure \
    --prefix=/usr \
    --with-intl=system-icu \
    --without-npm \
    --shared-openssl \
    --shared-zlib
    # --shared-libuv
    # --shared-v8
    # --shared-cares

  make
}

check() {
  cd node-$pkgver
  make test || warning "Tests failed"
}

package() {
  cd node-$pkgver

  make DESTDIR="$pkgdir" install

  # install docs as per user request
  install -d "$pkgdir"/usr/share/doc/nodejs
  cp -r doc/api/*.md \
    "$pkgdir"/usr/share/doc/nodejs

  install -D -m644 LICENSE \
    "$pkgdir"/usr/share/licenses/nodejs/LICENSE
}

# vim:set ts=2 sw=2 et:
