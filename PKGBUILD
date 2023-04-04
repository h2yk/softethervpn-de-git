# Maintainer: hayabusa2yk <me(at)h2yk(dot)dev>
# Contributor: Tun Win Naing <twnaing(at)outlook(dot)com>
# Contributor: Jonathan Liu <net147@gmail.com>
pkgname=softethervpn-de-git
pkgver=r1799.22c602f6
pkgrel=1
pkgdesc="Multi-protocol VPN Program from University of Tsukuba - Developer Edition"
arch=('any')
url="https://www.softether.org/"
license=('Apache')
depends=('bash' 'openssl' 'zlib' 'libsodium')
makedepends=('git' 'cmake')
conflicts=('softethervpn')
provides=('softethervpn')
source=("${pkgname}::git+https://github.com/SoftEtherVPN/SoftEtherVPN.git"
        'softethervpn-bridge.service'
        'softethervpn-client.service'
        'softethervpn-server.service')
md5sums=('SKIP' 
         '1d54c0065ae8947bd8455b9e2050c1af'
         'a1134fea991e6e00dc4910b1be16dc73'
         'b54b4f68d56555ddfffc50c2c399624f')

pkgver() {
  cd "$pkgname"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build(){
  cd "${srcdir}/${pkgname}"

  git submodule update --init --recursive
  ./configure
  make -C build
}

package(){
  cd "${srcdir}/${pkgname}"
  make -C build preinstall

  install -Dm444 build/hamcore.se2 "${pkgdir}"/usr/lib/softethervpn/hamcore.se2
  install -d "${pkgdir}"/usr/bin

  for inst in vpnclient vpnserver vpnbridge vpncmd; do
    install -Dm755 build/${inst} "${pkgdir}"/usr/lib/softethervpn/${inst}/${inst}
    ln -s /usr/lib/softethervpn/hamcore.se2 "${pkgdir}"/usr/lib/softethervpn/${inst}/hamcore.se2
    echo "#!/bin/sh" > "${pkgdir}"/usr/bin/${inst}
    echo /usr/lib/softethervpn/${inst}/${inst} '"$@"' >> "${pkgdir}"/usr/bin/${inst}
    echo 'exit $?' >> "${pkgdir}"/usr/bin/${inst}
    chmod 755 "${pkgdir}"/usr/bin/${inst}
  done

  for inst in libcedar.so libmayaqua.so; do
    install -Dm755 build/${inst} "${pkgdir}"/usr/lib/${inst}
  done

  install -d "${pkgdir}"/usr/lib/systemd/system
  install -Dm644 "${srcdir}"/*.service "${pkgdir}"/usr/lib/systemd/system
}

# vim:set ts=2 sw=2 et:
