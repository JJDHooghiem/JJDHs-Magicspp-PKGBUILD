# Contributor: Anton Bazhenov 
# Contributor: Graziano Giuliani
# My own build, orginal by the above contributors see their pkgbuild on the AUR
pkgname=magics++
Pkgname=Magics
pkgver=4.5.3
_attnum=3473464
pkgrel=2
pkgdesc="Magics is the latest generation of the ECMWF's Meteorological plotting software MAGICS."
arch=('i686' 'x86_64')
url="https://software.ecmwf.int/wiki/display/MAGP"
license=('Apache')
depends=('expat' 'qt5-base' 'proj' 'fftw' 'pango' 'netcdf-cxx-legacy' 'eccodes' 'python' 'libgeotiff')
optdepends=('libaec' 'odb_api')
makedepends=('boost' 'gcc-fortran'  'python' 'cmake' 'python-jinja')
source=(http://software.ecmwf.int/wiki/download/attachments/${_attnum}/${Pkgname}-${pkgver}-Source.tar.gz)
md5sums=('31e91178eaf6d18a348a740426409746')
  
 

build() {
  cd "$srcdir/${Pkgname}-${pkgver}-Source"
  rm -fr src/boost && ln -sf /usr/include/boost src
  [ -x /usr/bin/odb ] && has_odb=ON || has_odb=OFF
  # force this for now. Metview does not compile
  has_odb=OFF
  mkdir -p build
  cd build
  CC=gcc CXX='g++' \
  cmake -DCMAKE_LINKER_FLAGS="-pthread" \
    -DCMAKE_SHARED_LINKER_FLAGS="-pthread" \
    -DCMAKE_EXE_LINKER_FLAGS="-pthread" -DENABLE_ODB=${has_odb} \
    -DGEOTIFF_PATH=/usr -Dodb_api_DIR=/usr/share/odb_api/cmake \
    -DCMAKE_CXX_COMPILER=g++ -DCMAKE_CC_COMPILER=gcc \
    -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_BUILD_TYPE=production \
    -DCMAKE_INSTALL_DATADIR=/usr/share \
    -DENABLE_PYTHON=1\
    -DENABLE_METVIEW=1 -DENABLE_QT5=1 -DPYTHON_EXECUTABLE=/usr/bin/python3 ..
    # sed -i magics.pc-pkg-config-build.cmake -e '/REPLACE/s/++/\\\\+\\\\+/'
  make || return 1
}

package() {
  cd "$srcdir/${Pkgname}-${pkgver}-Source/build"
  make DESTDIR="$pkgdir" install || return 1
}
