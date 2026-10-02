pkgname=wine-daite-lite
pkgver=11.14
pkgrel=1
pkgdesc="Wine runtime with the ODBC/WoW64 fixes required by DAITE and Visual FoxPro"
arch=(x86_64)
url="https://www.winehq.org"
license=(LGPL-2.1-or-later)
provides=("wine=$pkgver")
conflicts=(wine wine-staging wine-nine)
options=(!strip !debug)

depends=(
  desktop-file-utils
  fontconfig
  freetype2
  gettext
  glib2
  glibc
  libgcc
  libpcap
  libunwind
  libx11
  libxcursor
  libxext
  libxkbcommon
  libxi
  libxrandr
  systemd-libs
  unixodbc
  wayland
)

optdepends=(
  'alsa-lib: audio support'
  'cups: printing support'
  'gnutls: TLS support'
  'libpulse: PulseAudio support'
  'wine-gecko: embedded HTML support'
  'wine-mono: .NET support'
)

source=(
  "wine-${pkgver}-1-x86_64.pkg.tar.zst::https://archive.archlinux.org/packages/w/wine/wine-${pkgver}-1-x86_64.pkg.tar.zst"
  'odbc32-i386.dll'
  'odbc32-x86_64.dll'
  'odbc32-x86_64.so'
  '0001-odbc-wow64-daite.patch'
)

noextract=("wine-${pkgver}-1-x86_64.pkg.tar.zst")

sha512sums=(
  '40b968ef454b3b5669f5bf3a1cb93da2c65f107bf909f6937743c1c4bee6a8713f8a4ceed525ee6ff2d2c3ff437858b9f2dec4b81df6d1e16eae1520ce52d9d2'
  '4bce4a975e407c1b18276d14c8c5594c8d037b00ccec0e0ab7a47bce59ace795f133dbbd3abf3b0fcbf04e1ec3a9de0207106210c64ceb306119d3ef67d42e48'
  '8878b387aa0f036a0021e63250f09e70feaa544fe650a5ef9c7cc45249613980b23a642fc214b8884d48a33e5ae0204dec8b30c21c7f9b7630d8587eee50b800'
  '76fe3fd3a2e51e6fb968124bdb8839daf3ab0ef3d6c873d028c21aef4d6439960f03537b88b179ef97466496aef1efb8503498f4323e6a7db86d2059fbad93b7'
  '8f5f5e0e22e82b1789d4bb7f7df0bd5edc99f67e8789a61639829e0e780400301a865d30b7b2b8f58110cf7f819f6e3be98597d8fd2998c2efc823c21dcf18ba'
)

package() {
  bsdtar --no-same-owner -xf "$srcdir/wine-${pkgver}-1-x86_64.pkg.tar.zst" -C "$pkgdir"

  rm -f "$pkgdir"/{.BUILDINFO,.INSTALL,.MTREE,.PKGINFO}

  # Development-only content is unnecessary for running DAITE.
  rm -rf "$pkgdir/usr/include" "$pkgdir/usr/share/man" "$pkgdir/usr/lib/pkgconfig"
  find "$pkgdir/usr/lib/wine" -type f -name '*.a' -delete
  rm -f \
    "$pkgdir/usr/bin/function_grep.pl" \
    "$pkgdir/usr/bin/widl" \
    "$pkgdir/usr/bin/winebuild" \
    "$pkgdir/usr/bin/winecpp" \
    "$pkgdir/usr/bin/winedump" \
    "$pkgdir/usr/bin/wineg++" \
    "$pkgdir/usr/bin/winegcc" \
    "$pkgdir/usr/bin/winemaker" \
    "$pkgdir/usr/bin/wmc" \
    "$pkgdir/usr/bin/wrc"

  install -m644 "$srcdir/odbc32-i386.dll" \
    "$pkgdir/usr/lib/wine/i386-windows/odbc32.dll"
  install -m644 "$srcdir/odbc32-x86_64.dll" \
    "$pkgdir/usr/lib/wine/x86_64-windows/odbc32.dll"
  install -m755 "$srcdir/odbc32-x86_64.so" \
    "$pkgdir/usr/lib/wine/x86_64-unix/odbc32.so"

  ln -s wine "$pkgdir/usr/bin/wine-daite"

  if [[ -f "$pkgdir/usr/share/licenses/wine/LICENSE" ]]; then
    install -Dm644 "$pkgdir/usr/share/licenses/wine/LICENSE" \
      "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  fi
}
