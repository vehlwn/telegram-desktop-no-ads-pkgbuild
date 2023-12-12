# https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop
pkgname=telegram-desktop-no-ads
pkgver=4.11.8
pkgrel=4
pkgdesc='Patched Telegram Desktop client without ads'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL3')
depends=('hunspell' 'ffmpeg' 'hicolor-icon-theme' 'lz4' 'minizip' 'openal' 'ttf-opensans'
         'qt6-imageformats' 'qt6-svg' 'qt6-wayland' 'xxhash'
         'rnnoise' 'pipewire' 'libxtst' 'libxrandr' 'jemalloc' 'abseil-cpp' 'libdispatch'
         'openssl' 'protobuf' 'glib2' 'libsigc++-3.0' 'glibmm-2.68')
makedepends=('cmake' 'git' 'ninja' 'python' 'range-v3' 'tl-expected' 'microsoft-gsl' 'meson'
             'extra-cmake-modules' 'wayland-protocols' 'plasma-wayland-protocols' 'libtg_owt'
             'gobject-introspection' 'boost' 'fmt' 'mm-common' 'perl-xml-parser' 'libsigc++-3.0'
             'mold' 'sccache')
optdepends=('webkit2gtk: embedded browser features'
            'xdg-desktop-portal: desktop integration')
conflicts=("telegram-desktop")
# Patches are from feature/remove-ads branch:
# https://github.com/vehlwn/tdesktop/tree/feature/remove-ads
source=(
    "https://github.com/telegramdesktop/tdesktop/releases/download/v${pkgver}/tdesktop-${pkgver}-full.tar.gz"
    "remove-ads.patch"
)
sha256sums=(
    "SKIP"
    "f5557b7808fbde75e7457bc82db6e2d359c7d65612d4887c63ef4641bcbfbf1b"
)

prepare() {
    cd tdesktop-$pkgver-full
    patch --forward --strip=1 -i "${srcdir}/remove-ads.patch"
}

build() {
    cd tdesktop-$pkgver-full
    cmake \
        -B build \
        -G Ninja \
        -DCMAKE_COLOR_DIAGNOSTICS=ON \
        -DCMAKE_INSTALL_PREFIX="/usr" \
        -DCMAKE_BUILD_TYPE=Release \
        -DTDESKTOP_API_ID=611335 \
        -DTDESKTOP_API_HASH=d524b414d21f4d37f08684c1df41ac9c
    cmake --build build
}

package() {
    cd tdesktop-$pkgver-full
    DESTDIR="$pkgdir" cmake --install build
}
