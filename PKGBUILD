pkgname=openssl
pkgver=3.5.2
pkgrel=1
pkgdesc="The Open Source toolkit for Secure Sockets Layer and Transport Layer Security"
arch=('x86_64')
url="https://www.openssl.org/"
license=('Apache-2.0')
depends=('glibc' 'zlib')
makedepends=('perl')
backup=(etc/ssl/openssl.cnf)
options=('!lto')
source=(https://www.openssl.org/source/${pkgname}-${pkgver}.tar.gz)
sha256sums=(c53a47e5e441c930c3928cf7bf6fb00e5d129b630e0aa873b08258656e7345ec)

build() {
    cd ${pkgname}-${pkgver}

    local configure_args=(
        --prefix=/usr
        --openssldir=/etc/ssl
        --libdir=lib64
        shared
        zlib-dynamic
        linux-x86_64
        enable-ec_nistp_64_gcc_128
    )

    CC="${CHOST}-gcc" ./config "${configure_args[@]}"

    make
}

package() {
    cd ${pkgname}-${pkgver}

    sed -i '/INSTALL_LIBS/s/libcrypto.a libssl.a//' Makefile

    make DESTDIR=${pkgdir} MANSUFFIX=ssl install

    rmdir ${pkgdir}/etc/ssl/certs

    mv -v ${pkgdir}/usr/share/doc/openssl ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}

    cp -vfr doc/* ${pkgdir}/usr/share/doc/${pkgname}-${pkgver}
}
