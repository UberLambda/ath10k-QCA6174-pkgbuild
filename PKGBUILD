# Author: Paolo Jovon <paolo.jovon@gmail.com>
_board="QCA6174"
pkgname=ath10k-QCA6174-git
pkgver=r282.ba0358e
pkgrel=1
pkgdesc="Replaces QCA6174 firmware in /lib/firmware/ath10k with the version at https://github.com/kvalo/ath10k-firmware."
arch=('any')
url="https://github.com/UberLambda/ath10k-QCA6174-pkgbuild"
license=('custom:LICENSE.qca_firmware')
depends=('linux-firmware')
makedepends=('git')
source=("git+https://github.com/kvalo/ath10k-firmware.git"
        "${pkgname}.install"
        "${pkgname}.hook")
sha1sums=('SKIP'
          '3bace43bb83795b86755e3380d19294615261950'
          'f83fd1a6a7d6a140009af87699603c06bc42eb73')
install="${pkgname}.install"

pkgver() {
  cd "${srcdir}/ath10k-firmware"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

#build() {
#}

package() {
    cd "${srcdir}/ath10k-firmware"

    install -D -m644 "LICENSE.qca_firmware" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE.qca_firmware"

    mkdir -p -m755 "${pkgdir}/usr/lib/firmware/atheros/${_board}"
    cp -r "${_board}" "${pkgdir}/usr/lib/firmware/atheros/"

    cd "${pkgdir}/usr/lib/firmware/atheros/${_board}"
    ln -s hw3.0/firmware-4.bin{_*,}
    ln -s hw2.1/firmware-5.bin{_*,}

    # Install PreTransaction hook
    install -D -m644 "${srcdir}/${pkgname}.hook" "${pkgdir}/etc/pacman.d/hooks/${pkgname}.hook"
}
