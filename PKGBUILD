# Maintainer: James An <james@jamesan.ca>
# Contributor: Sven Schneider <archlinux.sandmann@googlemail.com>

pkgbase=ompl
pkgname=omplapp-git
_pkgname=${pkgname%-git}
pkgver=r1339.bcf25ae
pkgrel=1
pkgdesc="The Open Motion Planning Library (OMPL) consists of many state-of-the-art sampling-based motion planning algorithms"
arch=('i686' 'x86_64')
url="http://ompl.kavrakilab.org"
license=('BSD')
depends=('boost' 'python' 'pyqt' 'python-opengl' 'assimp' 'pqp' 'libccd' 'fcl')
makedepends=('cmake' 'git')
optdepends=('py++: Python binding'
            'ode: Plan using the Open Dynamics Engine'
            'eigen: For an informed sampling technique')
provides=("$_pkgname=$pkgver")
conflicts=("$_pkgname")
source=("$_pkgname"::"git+https://github.com/$pkgbase/$_pkgname.git")
md5sums=('SKIP')

pkgver() {
    cd "$_pkgname"
    (
        set -o pipefail
        git describe --long --tag | sed -r 's/([^-]*-g)/r\1/;s/-/./g' ||
        printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
    )
}

prepare() {
  cd "$_pkgname"

  # pkg-config provides <LIB>_INCLUDEDIR instead of <LIB>_INCLUDE_DIRS
  sed 's/# Check for FCL and CCD installation, otherwise download them./set(CCD_INCLUDE_DIRS ${CCD_INCLUDEDIR})\nset(FCL_INCLUDE_DIRS ${FCL_INCLUDEDIR})/g' -i CMakeModules/FindFCL.cmake

  # (Re)create empty build folder
  [ -d build ] && rm -rf build &>/dev/null
  mkdir build
}

build() {
  cd "$_pkgname/build"

  cmake -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DOMPL_BUILD_PYBINDINGS=yes \
    -DPYTHON_EXEC=/usr/bin/python2 \
    -DOMPL_REGISTRATION=Off ..


  make
}

check() {
  cd "$_pkgname/build"

  make test
}

package() {
  cd "$_pkgname/build"

  make DESTDIR="$pkgdir" install
}

