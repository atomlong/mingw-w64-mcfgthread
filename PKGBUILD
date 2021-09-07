
pkgname=mingw-w64-mcfgthread
pkgver=20210616
pkgrel=1
pkgdesc="An efficient Gthread implementation for GCC (mingw-w64)"
arch=(any)
url="https://github.com/lhmouse/mcfgthread"
license=('LGPL')
depends=('mingw-w64-crt')
makedepends=('mingw-w64-configure')
options=(!strip !buildflags staticlibs)
source=(mcfgthread::"git+https://github.com/lhmouse/mcfgthread.git")
md5sums=('SKIP')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"

pkgver() {
  cd "${srcdir}/mcfgthread"
  git show -s --format="%ci" HEAD | sed -e 's/-//g' -e 's/ .*//'
}

build() {
  cd "${srcdir}/mcfgthread"
  autoreconf -vfi
  for _arch in ${_architectures}; do
    mkdir -p build-${_arch} && pushd build-${_arch}
    CFLAGS="-fno-exceptions" ${_arch}-configure ..
    make
    popd
  done
}

package() {
  for _arch in ${_architectures}; do
    cd "${srcdir}"/mcfgthread/build-${_arch}
    make DESTDIR="$pkgdir" install
    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
  done
}
