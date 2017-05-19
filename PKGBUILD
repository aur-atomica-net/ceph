# Maintainer: Jason R. McNeil <jason@jasonrm.net>

pkgname=ceph
pkgver=12.0.3
pkgrel=2
pkgdesc='Distributed, fault-tolerant storage platform delivering object, block, and file system'
arch=('x86_64')
url='https://ceph.com/'
license=('GPL')
makedepends=(
  'boost'
  'cmake'
  'cython'
  'cython2'
  'python'
  'python-sphinx'
  'python-virtualenv'
  'python2'
  'python2-virtualenv'
  'systemd'
  'xfsprogs'
  'yasm'
)
depends=(
  'boost-libs'
  'curl'
  'expat'
  'fcgi'
  'fuse2'
  'gcc-libs'
  'glibc'
  'gperftools'
  'jemalloc'
  'keyutils'
  'leveldb'
  'libaio'
  'libatomic_ops'
  'libedit'
  'libsystemd'
  'libutil-linux'
  'ncurses'
  'nss'
  'parted'
  'python2'
  'python-setuptools'
  'python2-setuptools'
  'python2-prettytable'
  'snappy'
)
optdepends=(
  'python: python bindings'
  'xfsprogs: support xfs backend'
)
options=('!emptydirs')
source=("https://download.ceph.com/tarballs/ceph-$pkgver.tar.gz"
        'ceph.sysusers')
sha256sums=('b0f2655c826bcba12c25828a8434f4e3d4cfdf41b5f59999da54132b96521de7'
            '69c5c1888c7f712b80e1f6b607f747a251a4f27734777721e8332e15f6bce785')


build() {
  cd $pkgname-$pkgver
  [[ -d build ]] || mkdir build
  cd build
  # list of options defaults: grep ^option CMakeLists.txt
  cmake \
    -DALLOCATOR=jemalloc \
    -DCMAKE_INSTALL_LIBDIR=/usr/lib \
    -DCMAKE_INSTALL_LIBEXECDIR=/usr/lib \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_SBINDIR=/usr/bin \
    -DCMAKE_INSTALL_SYSCONFDIR=/etc \
    -DHAVE_BABELTRACE=OFF \
    -DWITH_EMBEDDED=OFF \
    -DWITH_LTTNG=OFF \
    -DWITH_OPENLDAP=OFF \
    -DWITH_PYTHON3=ON \
    -DWITH_SYSTEM_BOOST=ON \
    -DWITH_SYSTEMD=ON \
    -DWITH_TESTS=ON \
    ..
  make
}

package() {
  cd $pkgname-$pkgver/build

  make DESTDIR="$pkgdir" install

  cd "$pkgdir"

  install -Dm644 "$srcdir"/$pkgname-$pkgver/systemd/ceph.tmpfiles.d usr/lib/tmpfiles.d/$pkgname.conf

  install -Dm644 "$srcdir"/ceph.sysusers usr/lib/sysusers.d/$pkgname.conf

  install -d -m 755 usr/lib/udev/rules.d
  cp "$srcdir"/$pkgname-$pkgver/udev/* usr/lib/udev/rules.d/

  # fix sbin path
  msg2 'Fix sbin paths'
  mv -v usr/sbin/* usr/bin

  # fix bash completions path
  msg2 'Fix bash completion path'
  install -d -m 755 usr/share/bash-completion
  mv -v etc/bash_completion.d usr/share/bash-completion/completions

  # remove debian init
  rm -v etc/init.d/ceph
}

# vim:set ts=2 sw=2 et:
