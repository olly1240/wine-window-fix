# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Eduardo Romero <eduardo@archlinux.org>
# Contributor: Giovanni Scafora <giovanni@archlinux.org>

pkgname=wine
pkgver=9.7
pkgrel=1

_pkgbasever=${pkgver/rc/-rc}

source=(https://dl.winehq.org/wine/source/9.x/$pkgname-$_pkgbasever.tar.xz{,.sign}
        30-win32-aliases.conf
        wine-binfmt.conf)
sha512sums=('0c15c3a0901162a386126f2dc987b276b379cc027fc72d9e31cf3122614742f876f30a5f24a495f546cd9c5bd5efce096bd9ca190d0f378fea3d42a298a06e80'
            'SKIP'
            '6e54ece7ec7022b3c9d94ad64bdf1017338da16c618966e8baf398e6f18f80f7b0576edf1d1da47ed77b96d577e4cbb2bb0156b0b11c183a0accf22654b0a2bb'
            'bdde7ae015d8a98ba55e84b86dc05aca1d4f8de85be7e4bd6187054bfe4ac83b5a20538945b63fb073caab78022141e9545685e4e3698c97ff173cf30859e285')
validpgpkeys=(5AC1A08B03BD7A313E0A955AF5E6E9EEB9461DD7
              DA23579A74D4AD9AF9D3F945CEFAC8EAAF17519D)

pkgdesc="A compatibility layer for running Windows programs"
url="https://www.winehq.org"
arch=(x86_64)
options=(staticlibs !lto)
license=(LGPL-2.1-or-later)
depends=(
  desktop-file-utils
  fontconfig      lib32-fontconfig
  freetype2       lib32-freetype2
  gcc-libs        lib32-gcc-libs
  gettext         lib32-gettext
  libpcap         lib32-libpcap
  libunwind       lib32-libunwind
  libxcursor      lib32-libxcursor
  libxkbcommon    lib32-libxkbcommon
  libxi           lib32-libxi
  libxrandr       lib32-libxrandr
  wayland         lib32-wayland
)
makedepends=(autoconf bison perl flex mingw-w64-gcc
  alsa-lib              lib32-alsa-lib
  gnutls                lib32-gnutls
  gst-plugins-base-libs lib32-gst-plugins-base-libs
  libcups               lib32-libcups
  libgphoto2
  libpulse              lib32-libpulse
  libxcomposite         lib32-libxcomposite
  libxcomposite         lib32-libxcomposite
  libxinerama           lib32-libxinerama
  libxxf86vm            lib32-libxxf86vm
  mesa                  lib32-mesa
  mesa-libgl            lib32-mesa-libgl
  opencl-headers
  opencl-icd-loader     lib32-opencl-icd-loader
  pcsclite              lib32-pcsclite
  samba
  sane
  sdl2                  lib32-sdl2
  unixodbc
  v4l-utils             lib32-v4l-utils
  vulkan-headers
  vulkan-icd-loader     lib32-vulkan-icd-loader
)
optdepends=(
  alsa-lib              lib32-alsa-lib
  alsa-plugins          lib32-alsa-plugins
  cups                  lib32-libcups
  dosbox
  gnutls                lib32-gnutls
  gst-plugins-bad
  gst-plugins-base      lib32-gst-plugins-base
  gst-plugins-base-libs lib32-gst-plugins-base-libs
  gst-plugins-good      lib32-gst-plugins-good
  gst-plugins-ugly
  libgphoto2
  libpulse              lib32-libpulse
  libxcomposite         lib32-libxcomposite
  libxinerama           lib32-libxinerama
  opencl-icd-loader     lib32-opencl-icd-loader
  pcsclite              lib32-pcsclite
  samba
  sane
  sdl2                  lib32-sdl2
  unixodbc
  v4l-utils             lib32-v4l-utils
  wine-gecko
  wine-mono
)
makedepends=(${makedepends[@]} ${depends[@]})
install=wine.install

build() {
  # Allow ccache to work
  mv $pkgname-$_pkgbasever $pkgname

  # Doesn't compile without remove these flags as of 4.10
  export CFLAGS="$CFLAGS -ffat-lto-objects"

  msg2 "Building Wine-64..."
  mkdir "$pkgname-64-build"
  cd "$pkgname-64-build"
  ../$pkgname/configure \
    --prefix=/usr \
    --libdir=/usr/lib \
    --with-x \
    --with-wayland \
    --with-gstreamer \
    --enable-win64

  make

  cd ..

  _wine32opts=(
    --libdir=/usr/lib32
    --with-wine64="$srcdir/$pkgname-64-build"
  )

  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  msg2 "Building Wine-32..."
  mkdir "$pkgname-32-build"
  cd "$pkgname-32-build"
  ../$pkgname/configure \
    --prefix=/usr \
    --with-x \
    --with-wayland \
    --with-gstreamer \
    "${_wine32opts[@]}"

  make
}

package() {
  msg2 "Packaging Wine-32..."
  cd "$srcdir/$pkgname-32-build"

  make prefix="$pkgdir/usr" \
    libdir="$pkgdir/usr/lib32" \
    dlldir="$pkgdir/usr/lib32/wine" install

  msg2 "Packaging Wine-64..."
  cd "$srcdir/$pkgname-64-build"
  make prefix="$pkgdir/usr" \
    libdir="$pkgdir/usr/lib" \
    dlldir="$pkgdir/usr/lib/wine" install

  # Font aliasing settings for Win32 applications
  install -d "$pkgdir"/usr/share/fontconfig/conf.{avail,default}
  install -m644 "$srcdir/30-win32-aliases.conf" "$pkgdir/usr/share/fontconfig/conf.avail"
  ln -s ../conf.avail/30-win32-aliases.conf "$pkgdir/usr/share/fontconfig/conf.default/30-win32-aliases.conf"
  install -Dm 644 "$srcdir/wine-binfmt.conf" "$pkgdir/usr/lib/binfmt.d/wine.conf"
}

# vim:set ts=8 sts=2 sw=2 et:
