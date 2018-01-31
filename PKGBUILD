# Maintainer: neozaru
# Original maintainer: redfish <redfish at galactica dot pw>

pkgname='monero-all-git-generic'
_monerover=0.11.1.0
pkgver=0.11.1.0.r189.g50fb2dd
pkgrel=1
arch=('x86_64')
url="https://getmonero.org/"
license=('custom:Cryptonote')

provides=('monero-wallet-gui' 'monero')
conflicts=('monero-wallet-gui')

depends=('openssl' 'boost-libs>=1.45' 'miniupnpc' 'unbound' 'libunwind' 'readline'
'qt5-base' 'qt5-declarative' 'qt5-graphicaleffects'
'qt5-location' 'qt5-quickcontrols' 'qt5-tools' 'qt5-webchannel'
'qt5-webengine' 'qt5-x11extras' 'qt5-xmlpatterns')

makedepends=('git' 'cmake' 'boost')

pkgdesc="Official QT GUI wallet for Monero, a private, secure, untraceable peer-to-peer currency"

_repourl=https://github.com/monero-project/monero-gui
source=("git+$_repourl")

md5sums=('SKIP')

_srcdir=monero-gui

pkgver() {
    cd "$srcdir/$_srcdir"
    git describe --long | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "$srcdir/$_srcdir"

    echo "var GUI_VERSION = \"$pkgver\"" > version.js
    echo "var GUI_MONERO_VERSION = \"$_monerover\"" >> version.js
}

build() {
    cd "$srcdir/$_srcdir"

    # TODO: Hack
    sed -i 's/cmake -D CMAKE_BUILD_TYPE=$CMAKE_BUILD_TYPE -D BUILD_GUI_DEPS=ON/cmake -D CMAKE_BUILD_TYPE=$CMAKE_BUILD_TYPE -D ARCH=x86-64 -D BUILD_GUI_DEPS=ON/g' get_libwallet_api.sh

    ./build.sh release
}

package() {
    install -D -m755 "$srcdir/$_srcdir/build/release/bin/monero-wallet-gui" "$pkgdir/usr/bin/monero-wallet-gui"
    install -D -m755 "$srcdir/$_srcdir/monero/build/release/bin/monero-wallet-cli" \
                     "$srcdir/$_srcdir/monero/build/release/bin/monero-wallet-rpc" \
                     "$srcdir/$_srcdir/monero/build/release/bin/monero-blockchain-export" \
                     "$srcdir/$_srcdir/monero/build/release/bin/monero-blockchain-import" \
                     "$srcdir/$_srcdir/monero/build/release/bin/monero-gen-trusted-multisig" \
                     "$srcdir/$_srcdir/monero/build/release/bin/monerod" \
                        "$pkgdir/usr/bin"

    install -D -m644 "$srcdir/$_srcdir/monero/utils/systemd/monerod.service" "$pkgdir/usr/lib/systemd/system/monerod.service"
    install -D -m644 "$srcdir/$_srcdir/monero/utils/conf/monerod.conf" "$pkgdir/etc/monerod.conf"
    install -D -m644 "$srcdir/$_srcdir/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
