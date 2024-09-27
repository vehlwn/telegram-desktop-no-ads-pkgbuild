# https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop
pkgname=telegram-desktop-no-ads
pkgver=5.5.5
pkgrel=1
pkgdesc='Patched Telegram Desktop client without ads'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL3')
depends=('hunspell' 'ffmpeg' 'hicolor-icon-theme' 'lz4' 'minizip' 'openal'
         'qt6-imageformats' 'qt6-svg' 'qt6-wayland' 'xxhash' 'ada'
         'rnnoise' 'pipewire' 'libxtst' 'libxrandr' 'libxcomposite' 'libxdamage' 'abseil-cpp' 'libdispatch'
         'openssl' 'protobuf' 'glib2' 'libsigc++-3.0' 'kcoreaddons' 'openh264')
makedepends=('cmake' 'git' 'ninja' 'python' 'range-v3' 'tl-expected' 'microsoft-gsl' 'meson'
             'extra-cmake-modules' 'wayland-protocols' 'plasma-wayland-protocols' 'libtg_owt'
             'gobject-introspection' 'boost' 'fmt' 'mm-common' 'perl-xml-parser' 'python-packaging'
             'glib2-devel'
         'mold')
optdepends=('webkit2gtk: embedded browser features'
            'xdg-desktop-portal: desktop integration')
conflicts=("telegram-desktop")
# Patches are from feature/remove-ads branch:
# https://github.com/vehlwn/tdesktop/tree/feature/remove-ads
# git format-patch upstream/dev..feature/remove-ads --stdout > remove-ads.patch
source=(
    "https://github.com/telegramdesktop/tdesktop/releases/download/v${pkgver}/tdesktop-${pkgver}-full.tar.gz"
    "remove-ads.patch"
    "telegram-desktop-5_5_5-fix_build_with_cppgir.patch"
)
sha256sums=(
    "SKIP"
    8766e5c8cad62dbedc37d007c4e93d81cd7ad1c8384bd8c0f15fb0adc25c8870
    ee54bdf8fe67c8fadfffc794763fc62f4c6a15eb535c80ba7b1b74d6ec178882
)

prepare() {
    cd tdesktop-$pkgver-full
    patch --forward --strip=1 -i "${srcdir}/remove-ads.patch"
    patch -Np1 -d "${srcdir}/tdesktop-$pkgver-full/cmake/external/glib/cppgir" -i "${srcdir}/telegram-desktop-5_5_5-fix_build_with_cppgir.patch"
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
