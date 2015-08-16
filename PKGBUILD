# Contributor: Arch Haskell Team <void@foobar.org>
# Contributor: Lex Black <autumn-wind at web dot de>

pkgname=haskell-dyre
pkgver=0.8.11
_hkgname=dyre
_licensefile=LICENSE
pkgrel=1
pkgdesc="Dynamic reconfiguration in Haskell"
url="http://github.com/willdonnelly/dyre"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=('ghc')
depends=('haskell-executable-path' 'haskell-ghc-paths' 'haskell-io-storage' 'haskell-xdg-basedir' 'haskell-old-time')
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
sha256sums=('286f5d03973dd3816e1c9178c6637793cab98ebe9a6871f0f66bd90fb566e569')
install="${pkgname}.install"

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O ${PKGBUILD_HASKELL_ENABLE_PROFILING:+-p } --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
