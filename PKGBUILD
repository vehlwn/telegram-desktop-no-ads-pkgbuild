# https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop
pkgname=telegram-desktop-no-ads
pkgver=4.8.1
pkgrel=6
pkgdesc='Patched Telegram Desktop client without ads'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL3')
depends=('hunspell' 'ffmpeg' 'hicolor-icon-theme' 'lz4' 'minizip' 'openal' 'ttf-opensans'
         'qt6-imageformats' 'qt6-svg' 'qt6-wayland' 'qt6-5compat' 'xxhash' 'glibmm-2.68'
         'rnnoise' 'pipewire' 'libxtst' 'libxrandr' 'jemalloc' 'abseil-cpp' 'libdispatch'
         'openssl' 'protobuf')
makedepends=('cmake' 'git' 'ninja' 'python' 'range-v3' 'tl-expected' 'microsoft-gsl' 'meson'
             'extra-cmake-modules' 'wayland-protocols' 'plasma-wayland-protocols' 'libtg_owt')
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
    "08d3062c8a0011b809d7b3551f97f52e7db69974b6b3324929f50cf71d5a9774"
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
