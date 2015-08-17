# Maintainer: Maxime Gauduin <alucryd@gmail.com>
# Contributor: josephgbr <rafael.f.f1@gmail.com>
# Contributor: Themaister <maister@archlinux.us>

pkgname=pcsx2-svn
pkgver=1.3.0.r5904
pkgrel=1
pkgdesc='A Sony PlayStation 2 emulator'
arch=('i686' 'x86_64')
url='http://www.pcsx2.net'
license=('GPL2' 'GPL3' 'LGPL2.1' 'LGPL3')
makedepends=('cmake' 'sparsehash' 'subversion')
if [[ $CARCH == i686 ]]; then
  depends=('glew' 'libaio' 'libcanberra' 'libjpeg-turbo' 'nvidia-cg-toolkit' 'portaudio' 'soundtouch' 'wxgtk2.8')
elif [[ $CARCH == x86_64 ]]; then
  depends=('lib32-glew' 'lib32-libaio' 'lib32-libcanberra' 'lib32-libjpeg-turbo' 'lib32-nvidia-cg-toolkit' 'lib32-portaudio' 'lib32-soundtouch' 'lib32-wxgtk2.8')
  makedepends+=('gcc-multilib')
  optdepends=('lib32-gtk-engines: GTK2 engines support'
              'lib32-gtk-murrine: murrine GTK3 engine support'
              'lib32-gtk-unico: unico GTK2 engine support')
fi
provides=("${pkgname%-*}")
conflicts=("${pkgname%-*}")
options=('!emptydirs')
source=("${pkgname%-*}::svn+http://pcsx2.googlecode.com/svn/trunk/")
sha256sums=('SKIP')

pkgver() {
  cd ${pkgname%-*}

  printf "1.3.0.r%s" "$(svnversion)"
}

build() {
  cd ${pkgname%-*}

  if [[ -d build ]]; then
    rm -rf build
  fi
  mkdir build && cd build

  if [[ $CARCH == i686 ]]; then
    cmake .. -DCMAKE_INSTALL_PREFIX='/usr' -DCMAKE_BUILD_TYPE='Release' -D{GLSL_API,PACKAGE_MODE,REBUILD_SHADER,XDG_STD}='ON' -DPLUGIN_DIR='/usr/lib/pcsx2' -DGAMEINDEX_DIR='/usr/share/pcsx2' -DwxWidgets_CONFIG_EXECUTABLE='/usr/bin/wx-config-2.8' -DwxWidgets_wxrc_EXECUTABLE='/usr/bin/wxrc-2.8'
  elif [[ $CARCH == x86_64 ]]; then
    export CC='gcc -m32'
    export CXX='g++ -m32'
    export PKG_CONFIG_PATH='/usr/lib32/pkgconfig'
    cmake .. -DCMAKE_INSTALL_PREFIX='/usr' -DCMAKE_BUILD_TYPE='Release' -D{GLSL_API,PACKAGE_MODE,REBUILD_SHADER,XDG_STD}='ON' -DPLUGIN_DIR='/usr/lib32/pcsx2' -DGAMEINDEX_DIR='/usr/share/pcsx2' -DwxWidgets_CONFIG_EXECUTABLE='/usr/bin/wx-config32-2.8' -DwxWidgets_wxrc_EXECUTABLE='/usr/bin/wxrc32-2.8' -DCMAKE_LIBRARY_PATH='/usr/lib32'
  fi
  make
}

package() {
  cd ${pkgname%-*}/build

  make DESTDIR="${pkgdir}" install
}

# vim: ts=2 sw=2 et:
