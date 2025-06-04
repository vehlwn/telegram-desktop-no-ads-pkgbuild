# https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop
pkgname=telegram-desktop-no-ads
pkgver=5.14.3
pkgrel=2
pkgdesc='Patched Telegram Desktop client without ads'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL3')
depends=(
  'abseil-cpp'
  'ada'
  'ffmpeg'
  'glib2'
  'hicolor-icon-theme'
  'hunspell'
  'kcoreaddons'
  'libdispatch'
  'libxcomposite'
  'libxdamage'
  'libxrandr'
  'libxtst'
  'lz4'
  'minizip'
  'openal'
  'openh264'
  'openssl'
  'pipewire'
  'protobuf'
  'qt6-imageformats'
  'qt6-svg'
  'qt6-wayland'
  'rnnoise'
  'xxhash'
)
makedepends=(
  'boost'
  'cmake'
  'git'
  'glib2-devel'
  'gobject-introspection'
  'gperf'
  'libtg_owt'
  'microsoft-gsl'
  'ninja'
  'python'
  'range-v3'
  'tl-expected'
  'mold'
)
optdepends=(
  'geoclue: geoinformation support'
  'geocode-glib-2: geocoding support'
  'geocode-glib: geocoding support'
  'webkit2gtk-4.1: embedded browser features provided by webkit2gtk-4.1'
  'webkit2gtk: embedded browser features provided by webkit2gtk'
  'webkitgtk-6.0: embedded browser features provided by webkitgtk-6.0 (Wayland only)'
  'xdg-desktop-portal: desktop integration'
)
conflicts=("telegram-desktop")
_td_commit=51743dfd01dff6179e2d8f7095729caa4e2222e9
# Patches are from feature/remove-ads branch:
# https://github.com/vehlwn/tdesktop/tree/feature/remove-ads
# git format-patch upstream/dev..feature/remove-ads --stdout > remove-ads.patch
source=(
    "https://github.com/telegramdesktop/tdesktop/releases/download/v${pkgver}/tdesktop-${pkgver}-full.tar.gz"
    "remove-ads.patch"
    "git+https://github.com/tdlib/td.git#tag=${_td_commit}"
)
sha256sums=(
    "SKIP"
    da2eee3925ea3cd5d202e0018270a6b44c2ca3f06db37bb941acf3c80aca00ee
    "SKIP"
)

prepare() {
    cd tdesktop-$pkgver-full
    patch --forward --strip=1 -i "${srcdir}/remove-ads.patch"
}

build() {
    cmake -S td -B td/build \
        -DCMAKE_BUILD_TYPE=None \
        -DCMAKE_INSTALL_PREFIX="$PWD/td/install" \
        -Wno-dev \
        -DTD_E2E_ONLY=ON
    cmake --build td/build
    cmake --install td/build

    cmake -B build -S tdesktop-$pkgver-full -G Ninja \
        -DCMAKE_INSTALL_PREFIX="/usr" \
        -Dtde2e_DIR="$PWD/td/install/lib/cmake/tde2e" \
        -DCMAKE_BUILD_TYPE=Release \
        -DTDESKTOP_API_ID=611335 \
        -DTDESKTOP_API_HASH=d524b414d21f4d37f08684c1df41ac9c
    cmake --build build
}

package() {
    DESTDIR="$pkgdir" cmake --install build
}
