# Author: Paolo Jovon <paolo.jovon@gmail.com>

_board="QCA6174"
_hw3s=('2.0' '4.4.1' 'sdio-4.4.1')

pkgname=ath10k-QCA6174-git
pkgver=r369.84b4706
pkgrel=1
pkgdesc="Replaces QCA6174 firmware in /lib/firmware/ath10k with the version at https://github.com/kvalo/ath10k-firmware."
arch=('any')
url="https://github.com/UberLambda/ath10k-QCA6174-pkgbuild"
license=('custom:LICENSE.qca_firmware')
depends=('linux-firmware')
makedepends=('git' 'sed')
source=("git+https://github.com/kvalo/ath10k-firmware.git"
        "${pkgname}.install"
        "${pkgname}.hook")
sha1sums=('SKIP'
          '3bace43bb83795b86755e3380d19294615261950'
          'd2ac38e2d7849767ff3ceda0f81de0452a855133')
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

    mkdir -p -m755 "${pkgdir}/usr/lib/firmware/ath10k/${_board}"
    pushd "${pkgdir}/usr/lib/firmware/ath10k/${_board}"

    # hw2.1: No subfolders; firmware-5.bin has a _suffix that needs removed
    cp -r "${srcdir}/ath10k-firmware/${_board}/hw2.1" "hw2.1"
    mv hw2.1/firmware-5.bin{_*,}

    # hw3.0: One subfolder per firmware version (?); duplicates are present.
    # _suffix after .bind needs removed
    install -D -m644 "${srcdir}/ath10k-firmware/${_board}/hw3.0/board-2.bin" "hw3.0/board-2.bin"
    for HW3 in ${_hw3s[@]}; do
        HW3_SRCDIR="${srcdir}/ath10k-firmware/${_board}/hw3.0/${HW3}"

        LATEST_BIN_SRCFILE=$(ls ${HW3_SRCDIR}/firmware-*.bin* | sort -r | head -n1)
        LATEST_BIN_NAME=$(basename $LATEST_BIN_SRCFILE | sed 's/\.bin_.*/\.bin/g')
        LATEST_BIN_DSTFILE="hw3.0/${LATEST_BIN_NAME}"
        echo "-- ${LATEST_BIN_NAME} -> ${LATEST_BIN_DSTFILE}"
        install -D -m644 "$LATEST_BIN_SRCFILE" "hw3.0/$LATEST_BIN_NAME"

        LATEST_NOTICE_NAME=$(basename $LATEST_BIN_SRCFILE | sed -r 's/.+\.bin(_.*)/notice\.txt\1/g')
        install -D -m644 "$HW3_SRCDIR/$LATEST_NOTICE_NAME" "hw3.0/$LATEST_NOTICE_NAME"
    done

    # Install PreTransaction hook
    install -D -m644 "${srcdir}/${pkgname}.hook" "${pkgdir}/etc/pacman.d/hooks/${pkgname}.hook"
}
