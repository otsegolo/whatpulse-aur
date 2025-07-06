# Maintainer: You, now. Spread those cheeks, you own it.
pkgname=whatpulse
pkgver=latest
pkgrel=1
pkgdesc="Measures your keyboard, mouse, app usage, network traffic, uptime. Desktop statwhore vibes."
arch=('x86_64')
url="https://www.whatpulse.org"
license=('custom:whatpulse_tos')

depends=(
    freetype2 xcb-util-image libxkbcommon libxkbcommon-x11 xcb-util-renderutil gcc-libs dbus krb5
    xcb-util-wm glib2 libx11 fontconfig libglvnd xcb-util-keysyms openssl-1.1 glibc libxcb zlib
    hicolor-icon-theme
)

makedepends=(imagemagick patchelf)
optdepends=('libpcap: for capturing network statistics')

source=(
    "whatpulse.desktop"
    "whatpulse.sh"
    "LICENSE"
)
source_x86_64=("${pkgname}.AppImage::https://releases.whatpulse.org/latest/linux/whatpulse-linux-latest_amd64.AppImage")

sha256sums=('SKIP' 'SKIP' 'SKIP')
sha256sums_x86_64=('SKIP')

prepare() {
    chmod +x "${pkgname}.AppImage"
    ./"${pkgname}.AppImage" --appimage-extract
    mv squashfs-root sfs

    # Patch binaries so Qt doesn't whine and projectile vomit
    find sfs/usr/{bin,lib,plugins} -type f -exec \
        patchelf --set-rpath '/usr/lib/whatpulse/lib:/usr/lib' '{}' \;
}

package() {
    # Launcher script & binary
    install -Dm755 whatpulse.sh "${pkgdir}/usr/bin/whatpulse"
    install -Dm755 sfs/usr/bin/whatpulse "${pkgdir}/usr/lib/whatpulse/whatpulse"

    # Libs
    find sfs/usr/lib -type f -exec \
        install -Dm644 '{}' "${pkgdir}/usr/lib/whatpulse/lib/$(basename '{}')" \;

    # Qt plugins (all of them, we’re not here to micromanage)
    find sfs/usr/plugins -type f -exec \
        install -Dm644 '{}' "${pkgdir}/usr/lib/whatpulse/plugins/${_relpath="${_f#sfs/usr/plugins/}"}" \;

    # Desktop entry
    install -Dm644 whatpulse.desktop "${pkgdir}/usr/share/applications/whatpulse.desktop"

    # License (is more like a TOS, but it slaps you legally so here we go)
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

    # Icon generation — fully fixed crop dance
    for size in 16 20 22 24 28 32 36 44 48 64 72 96 128 150 192 256 310 384 512 1024; do
        install -dm755 "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps"
        magick \
            sfs/whatpulse.png \
            -resize "${size}x${size}" \
            -background none -gravity center -extent "${size}x${size}" \
            "${pkgdir}/usr/share/icons/hicolor/${size}x${size}/apps/whatpulse.png"
    done
}
