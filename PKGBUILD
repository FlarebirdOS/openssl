pkgname=openssl
pkgver=3.5.2
pkgrel=2
pkgdesc="The Open Source toolkit for Secure Sockets Layer and Transport Layer Security"
arch=('x86_64')
url="https://www.openssl.org/"
license=('Apache-2.0')
depends=(
    'glibc'
    'zlib'
)
makedepends=('perl')
backup=(etc/ssl/openssl.cnf)
source=(https://www.openssl.org/source/${pkgname}-${pkgver}.tar.gz
    ca-dir.patch)
sha256sums=(c53a47e5e441c930c3928cf7bf6fb00e5d129b630e0aa873b08258656e7345ec
    0a32d9ca68e8d985ce0bfef6a4c20b46675e06178cc2d0bf6d91bd6865d648b7)

prepare() {
    cd ${pkgname}-${pkgver}

    # set ca dir to /etc/ssl by default
    patch -Np1 -i ${srcdir}/ca-dir.patch
}

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --prefix=/usr
        --openssldir=/etc/ssl
        --libdir=lib64
        shared
        zlib-dynamic
        enable-ktls
        linux-x86_64
        enable-ec_nistp_64_gcc_128
    )

    CC="${CHOST}-gcc" ./config "${configure_args[@]}"

    make depend
    make
}

package() {
    cd ${pkgname}-${pkgver}

    sed -i '/INSTALL_LIBS/s/libcrypto.a libssl.a//' Makefile

    make DESTDIR=${pkgdir} MANDIR=/usr/share/man MANSUFFIX=ssl install_sw install_ssldirs install_man_docs

    # rmdir ${pkgdir}/etc/ssl/certs

    # mv -v ${pkgdir}/usr/share/doc/openssl ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}

    # cp -vfr doc/* ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}
}
