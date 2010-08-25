# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Eduardo Romero <eduardo@archlinux.org>
# Contributor: Giovanni Scafora <giovanni@archlinux.org>

pkgname=wine
pkgver=1.3.1
pkgrel=2

_pkgbasever=${pkgver/rc/-rc}

source=(http://ibiblio.org/pub/linux/system/emulators/$pkgname/$pkgname-$_pkgbasever.tar.bz2)
md5sums=('5be30df05787f5db5bd5d2aa154aee87')

pkgdesc="A compatibility layer for running Windows programs"
url="http://www.winehq.com"
arch=(i686 x86_64)
license=(LGPL)

depends=(
  fontconfig      lib32-fontconfig
  mesa            lib32-mesa 
  libxcursor      lib32-libxcursor
  libxrandr       lib32-libxrandr
  libxdamage      lib32-libxdamage
  libxxf86dga     lib32-libxxf86dga
  alsa-lib        lib32-alsa-lib
)

makedepends=(autoconf ncurses bison perl fontforge flex prelink
  'gcc>=4.5.0-2'  'gcc-multilib>=4.5.0-2'
  giflib          lib32-giflib
  libxpm          lib32-libxpm
  libpng          lib32-libpng
  libxinerama     lib32-libxinerama
  libxcomposite   lib32-libxcomposite
  libxmu          lib32-libxmu
  libxxf86vm      lib32-libxxf86vm
  libxml2         lib32-libxml2
  libxslt         lib32-libxslt
  libldap         lib32-libldap
  lcms            lib32-lcms
  mpg123          lib32-mpg123
  openal          lib32-openal
  esound          lib32-esound
  jack            lib32-jack
  libcups         lib32-libcups
  libgphoto2
  sane
  smbclient 
)
  
optdepends=(
  giflib        lib32-giflib
  libpng        lib32-libpng
  libldap       lib32-libldap
  lcms          lib32-lcms
  libxml2       lib32-libxml2
  mpg123        lib32-mpg123
  openal        lib32-openal
  esound        lib32-esound
  jack          lib32-jack
  libcups       lib32-libcups
  libgphoto2
  sane
  smbclient
)

if [[ $CARCH == i686 ]]; then
  # Strip lib32 etc. on i686
  depends=(${depends[@]/*32-*/})
  makedepends=(${makedepends[@]/*32-*/})
  makedepends=(${makedepends[@]/*-multilib*/})
  optdepends=(${optdepends[@]/*32-*/})
else
  provides=("bin32-wine=$pkgver" "wine-wow64=$pkgver")
  conflicts=('bin32-wine' 'wine-wow64')
  replaces=('bin32-wine')
fi

build() {
  cd "$srcdir"

  # Allow ccache to work
  mv $pkgname-$_pkgbasever $pkgname

  # Get rid of old build dirs
  rm -rf $pkgname-{32,64}-build
  mkdir $pkgname-32-build

  if [[ $CARCH == x86_64 ]]; then
    msg2 "Building Wine-64..."

    mkdir $pkgname-64-build
    cd "$srcdir/$pkgname-64-build"
    ../$pkgname/configure \
      --prefix=/usr \
      --sysconfdir=/etc \
      --libdir=/usr/lib \
      --with-x \
      --enable-win64

    make

    # Wine32 build needs a patched ldd
    # Do it here until it's patched in glibc
    cp /usr/bin/ldd "$srcdir/ldd"
    sed -i '/^RTLDLIST=/s:"$: /lib/ld-linux.so.2&:' "$srcdir/ldd"

    _wine32opts=(
      LDD="$srcdir/ldd"
      --libdir=/usr/lib32
      --with-wine64="$srcdir/$pkgname-64-build"
    )

    export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"
  fi

  msg2 "Building Wine-32..."
  cd "$srcdir/$pkgname-32-build"
  ../$pkgname/configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --with-x \
    "${_wine32opts[@]}"

  make
}

package() {
  msg2 "Packaging Wine-32..."
  cd "$srcdir/$pkgname-32-build"

  if [[ $CARCH == i686 ]]; then
    make prefix="$pkgdir/usr" install
  else
    make prefix="$pkgdir/usr" \
      libdir="$pkgdir/usr/lib32" \
      dlldir="$pkgdir/usr/lib32/wine" install

    msg2 "Packaging Wine-64..."
    cd "$srcdir/$pkgname-64-build"
    make prefix="$pkgdir/usr" \
      libdir="$pkgdir/usr/lib" \
      dlldir="$pkgdir/usr/lib/wine" install
  fi

  mkdir -p "$pkgdir/etc/wine"
}

# vim:set ts=8 sts=2 sw=2 et:
