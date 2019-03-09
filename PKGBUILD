# Author: Paolo Jovon <paolo.jovon@gmail.com>
pkgname=ath10k-QCA6174-git
pkgver=r281.539d3f8
pkgrel=1
pkgdesc="Replaces QCA6174 fiwmare in /lib/firmware/ath10k with the version at https://github.com/kvalo/ath10k-firmware."
arch=('any')
url="https://github.com/kvalo/ath10k-firmware"
license=('custom:LICENSE.qca_firmware')
depends=('linux-firmware')
makedepends=('git')
source=("git+https://github.com/kvalo/ath10k-firmware.git")
sha1sums=('SKIP')
install="${pkgname}.install"
_board="QCA6174"

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

}
