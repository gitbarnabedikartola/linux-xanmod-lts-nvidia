# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

# Arch credits:
# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod-lts
_extramodules=$(find /usr/lib/modules -type d -iname 5.15.89*xanmod* | rev | cut -d "/" -f1 | rev)

pkgname=$_linuxprefix-nvidia
pkgdesc="NVIDIA drivers for linux"
pkgver=525.85.05
pkgrel=515891
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix" "nvidia-utils=$pkgver")
makedepends=("$_linuxprefix-headers")
provides=("nvidia=$pkgver" 'NVIDIA-MODULE')
options=(!strip)
install=nvidia.install
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
sha256sums=('a6109c2fc1d271185f5ae2b997dc68c93af43e1583143bf80e254290a9a3b039')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}"
    # patches here
}

build() {
    _kernver=$(find /usr/lib/modules -type d -iname 5.15.89*xanmod* | rev | cut -d "/" -f1 | rev)

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    cd "${_pkg}"
    install -Dm644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_extramodules}/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}