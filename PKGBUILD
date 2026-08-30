# https://gitlab.archlinux.org/archlinux/packaging/packages/telegram-desktop
pkgname=telegram-desktop-no-ads
pkgver=7.1.3
_td_commit=bc9c263e2bfee06aaab41e82db51a103376030bc
pkgrel=1
pkgdesc='Patched Telegram Desktop client without ads'
arch=('x86_64')
url="https://desktop.telegram.org/"
license=('GPL3')
depends=(
  'abseil-cpp'
  'ada'
  'ffmpeg'
  'glib2'
  'glibc'
  'hicolor-icon-theme'
  'hunspell'
  'kcoreaddons'
  'libavif'
  'libfido2'
  'libgcc'
  'libheif'
  'libjpeg-turbo'
  'libjxl'
  'libpipewire'
  'libsrtp'
  'libstdc++'
  'libxcb'
  'libxcomposite'
  'libxdamage'
  'libxext'
  'libxfixes'
  'libxkbcommon'
  'libxrandr'
  'libxtst'
  'lz4'
  'minizip'
  'openal'
  'openh264'
  'openssl'
  'pipewire'
  'qt6-base'
  'qt6-imageformats'
  'qt6-svg'
  'qt6-wayland'
  'rnnoise'
  'xxhash'
  'zlib'
)
makedepends=(
  'boost'
  'boost-libs'
  'cmake'
  'git'
  'glib2-devel'
  'gobject-introspection'
  'qt6-shadertools'
  'gperf'
  'libtg_owt'
  'microsoft-gsl'
  'ninja'
  'python'
  'range-v3'
  'tl-expected'
  'vulkan-headers'
)
conflicts=("telegram-desktop")

# Patches are from feature/remove-ads branch:
# https://github.com/vehlwn/tdesktop/tree/feature/remove-ads
# git format-patch upstream/dev..feature/remove-ads --stdout > remove-ads.patch
source=(
    "https://github.com/telegramdesktop/tdesktop/releases/download/v${pkgver}/tdesktop-${pkgver}-full.tar.gz"
    "git+https://github.com/tdlib/td.git#tag=${_td_commit}"
    "remove-ads.patch"
)
sha256sums=(
    "SKIP"
    "SKIP"
    5c3c35f0784b6d9d103a535bb99f4cb1cd432d0dcbe262f6a84230289902021e
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
