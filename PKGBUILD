# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Jan "heftig" Steffens <jan.steffens@gmail.com>
# Contributor: Eduardo Romero <eduardo@archlinux.org>
# Contributor: Giovanni Scafora <giovanni@archlinux.org>

pkgname=wine-osu
pkgver=7.0_staging
pkgrel=1

_pkgbasever=7.0

#source=("git+https://github.com/wine-mirror/wine.git#tag=wine-$_pkgbasever"
source=(https://dl.winehq.org/wine/source/7.0/wine-$_pkgbasever.tar.xz{,.sign}
        "https://github.com/wine-staging/wine-staging/archive/v$_pkgbasever/wine-staging-v$_pkgbasever.tar.gz"
        30-win32-aliases.conf
        wine-binfmt.conf
        winepulse-513.tar
        winepulse-v6.11-0002-5.14-Latency-Fix.patch
        )
noextract=(winepulse-513.tar)
sha512sums=('eec17b046ed5447eb540f421c9b2748d9419ce087496c2743a9914fd27bbe5ff9da0cfe47d3cd76fa97323bd1188a1d82b1eef4968d86ed1957dc1a95e28529c'
            'SKIP'
            'fbec2de7a13c7e59a041d8102d69b803d4475b743068d215cce510af905b81903aa028604068af0d309fe1708eb1ab62aad42887ac079af5206635bee0045952'
            '6e54ece7ec7022b3c9d94ad64bdf1017338da16c618966e8baf398e6f18f80f7b0576edf1d1da47ed77b96d577e4cbb2bb0156b0b11c183a0accf22654b0a2bb'
            'bdde7ae015d8a98ba55e84b86dc05aca1d4f8de85be7e4bd6187054bfe4ac83b5a20538945b63fb073caab78022141e9545685e4e3698c97ff173cf30859e285'
            '73e626c0cb41f1e4c28ef4b78e9d78f4ce889757c02fb1bfd119c3077c540d71f720ec5a284904f0055b5d83425a26a315d738ae38ddea5964e7fc7d4742c921'
            'e054ada5cce69b5fe327726a31012e7fd4ce1d986e6ca73ca8bf5dc035d185af84b2c9ad7034b7b24a190f6849dbf4dfccc2de5d169b4eb2abc4c2a7b18e68e9')
validpgpkeys=(5AC1A08B03BD7A313E0A955AF5E6E9EEB9461DD7
              DA23579A74D4AD9AF9D3F945CEFAC8EAAF17519D)

pkgdesc="A compatibility layer for running Windows programs"
provides=(wine-osu)
conflicts=(wine-osu)
url="http://www.winehq.com"
arch=(x86_64)
options=(staticlibs)
license=(LGPL)
depends=(
  fontconfig      lib32-fontconfig
  lcms2           lib32-lcms2
  libxml2         lib32-libxml2
  libxcursor      lib32-libxcursor
  libxrandr       lib32-libxrandr
  libxdamage      lib32-libxdamage
  libxi           lib32-libxi
  gettext         lib32-gettext
  freetype2       lib32-freetype2
  glu             lib32-glu
  libsm           lib32-libsm
  gcc-libs        lib32-gcc-libs
  libpcap         lib32-libpcap
  faudio          lib32-faudio
  desktop-file-utils
)
makedepends=(autoconf bison perl fontforge flex mingw-w64-gcc
  giflib                lib32-giflib
  libpng                lib32-libpng
  gnutls                lib32-gnutls
  libxinerama           lib32-libxinerama
  libxcomposite         lib32-libxcomposite
  libxmu                lib32-libxmu
  libxxf86vm            lib32-libxxf86vm
  libldap               lib32-libldap
  mpg123                lib32-mpg123
  openal                lib32-openal
  v4l-utils             lib32-v4l-utils
  libpulse              lib32-libpulse
  alsa-lib              lib32-alsa-lib
  libxcomposite         lib32-libxcomposite
  mesa                  lib32-mesa
  mesa-libgl            lib32-mesa-libgl
  opencl-icd-loader     lib32-opencl-icd-loader
  libxslt               lib32-libxslt
  gst-plugins-base-libs lib32-gst-plugins-base-libs
  vulkan-icd-loader     lib32-vulkan-icd-loader
  vkd3d                 lib32-vkd3d
  sdl2                  lib32-sdl2
  libcups               lib32-libcups
  libgphoto2
  sane
  gsm
  vulkan-headers
  samba
  opencl-headers
)
optdepends=(
  giflib                lib32-giflib
  libpng                lib32-libpng
  libldap               lib32-libldap
  gnutls                lib32-gnutls
  mpg123                lib32-mpg123
  openal                lib32-openal
  v4l-utils             lib32-v4l-utils
  libpulse              lib32-libpulse
  alsa-plugins          lib32-alsa-plugins
  alsa-lib              lib32-alsa-lib
  libjpeg-turbo         lib32-libjpeg-turbo
  libxcomposite         lib32-libxcomposite
  libxinerama           lib32-libxinerama
  opencl-icd-loader     lib32-opencl-icd-loader
  libxslt               lib32-libxslt
  gst-plugins-base-libs lib32-gst-plugins-base-libs
  vkd3d                 lib32-vkd3d
  sdl2                  lib32-sdl2
  libgphoto2
  sane
  gsm
  cups
  samba           dosbox
)
makedepends=(${makedepends[@]} ${depends[@]})
install=wine.install

prepare() {
  # Allow ccache to work
  #mv wine $pkgname
  mv wine-$_pkgbasever $pkgname

  # apply wine-staging patchset
  pushd wine-staging-$_pkgbasever/patches
  ./patchinstall.sh DESTDIR="$srcdir/$pkgname" --all
  popd

  # Doesn't compile without remove these flags as of 4.10
  export CFLAGS="${CFLAGS/-fno-plt/}"
  export LDFLAGS="${LDFLAGS/,-z,now/}"

  if [ -f "$srcdir"/winepulse-513.tar ]; then
    cd "$srcdir"/"$pkgname"/dlls/winepulse.drv
    rm ./*
    tar xf "$srcdir"/winepulse-513.tar
    echo extracted winepulse-513.tar
    cd -
  fi

  cd "$srcdir"/$pkgname
  echo $PWD
  for i in "${srcdir}"/*patch; do
    [ ! -f "$i" ] && continue
    echo "applying '$i'"
    #git am "$i"
    patch -Np1 -i "$i"
  done
  autoconf
  cd "$OLDPWD"

  sed 's|OpenCL/opencl.h|CL/opencl.h|g' -i $pkgname/configure*

  # Get rid of old build dirs
  rm -rf $pkgname-{32,64}-build
  mkdir $pkgname-{32,64}-build
}

build() {
  cd "$srcdir"

  msg2 "Building Wine-64..."

  cd "$srcdir/$pkgname-64-build"
  ../$pkgname/configure \
    --prefix=/opt/$pkgname \
    --libdir=/opt/$pkgname/lib \
    --with-x \
    --with-gstreamer \
    --enable-win64 \
    --enable-silent-rules
  # Gstreamer was disabled for FS#33655

  make -j4

  _wine32opts=(
    --libdir=/opt/$pkgname/lib32
    --with-wine64="$srcdir/$pkgname-64-build"
  )

  export PKG_CONFIG_PATH="/usr/lib32/pkgconfig"

  msg2 "Building Wine-32..."
  cd "$srcdir/$pkgname-32-build"
  ../$pkgname/configure \
    --prefix=/opt/$pkgname \
    --with-x \
    --with-gstreamer \
    --enable-silent-rules \
    "${_wine32opts[@]}"

  make -j4 --quiet
}

package() {
  msg2 "Packaging Wine-32..."
  cd "$srcdir/$pkgname-32-build"

  make prefix="$pkgdir/opt/$pkgname" \
    libdir="$pkgdir/opt/$pkgname/lib32" \
    dlldir="$pkgdir/opt/$pkgname/lib32/wine" install

  msg2 "Packaging Wine-64..."
  cd "$srcdir/$pkgname-64-build"
  make prefix="$pkgdir/opt/$pkgname" \
    libdir="$pkgdir/opt/$pkgname/lib" \
    dlldir="$pkgdir/opt/$pkgname/lib/wine" install

  # Font aliasing settings for Win32 applications
  install -d "$pkgdir"/opt/$pkgname/share/fontconfig/conf.{avail,default}
  install -m644 "$srcdir/30-win32-aliases.conf" "$pkgdir/opt/$pkgname/share/fontconfig/conf.avail"
  ln -s ../conf.avail/30-win32-aliases.conf "$pkgdir/opt/$pkgname/share/fontconfig/conf.default/30-win32-aliases.conf"
  install -Dm 644 "$srcdir/wine-binfmt.conf" "$pkgdir/opt/$pkgname/lib/binfmt.d/wine.conf"

  i686-w64-mingw32-strip --strip-unneeded "$pkgdir"/opt/$pkgname/lib32/wine/i386-windows/*.dll
  x86_64-w64-mingw32-strip --strip-unneeded "$pkgdir"/opt/$pkgname/lib/wine/x86_64-windows/*.dll
}

# vim:set ts=8 sts=2 sw=2 et:
