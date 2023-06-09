# Maintainer : ehmish
pkgdesc="ROS - RTAB-Maps standalone library."
url='http://introlab.github.io/rtabmap'

pkgname='ros-noetic-rtabmap'
pkgver='0.21.1'
_pkgver_patch=1
arch=('any')
pkgrel=1
license=('BSD')

ros_makedepends=(ros-noetic-octomap
  ros-noetic-qt-gui-cpp
  ros-noetic-cv-bridge)
makedepends=('cmake' 'ros-build-tools'
  ${ros_makedepends[@]}
  sqlite
  vtk
  zlib
  proj
  pcl
)

ros_depends=(ros-noetic-octomap
  ros-noetic-qt-gui-cpp
  ros-noetic-cv-bridge)
depends=(${ros_depends[@]}
  sqlite
  vtk
  zlib
  pcl
  )

# Git version (e.g. for debugging)
# _tag=release/noetic/rtabmap/${pkgver}-${_pkgver_patch}
# _dir=${pkgname}
# source=("${_dir}"::"git+https://github.com/introlab/rtabmap-release.git"#tag=${_tag})
# sha256sums=('SKIP')

# Tarball version (faster download)
_dir="rtabmap-release-release-noetic-rtabmap-${pkgver}-${_pkgver_patch}"
#_dir="rtabmap-release-release-noetic-rtabmap"
source=(
	"${pkgname}-${pkgver}-${_pkgver_patch}.tar.gz"::"https://github.com/introlab/rtabmap-release/archive/release/noetic/rtabmap/${pkgver}-${_pkgver_patch}.tar.gz"
	fix-python-scripts.sh
)
sha256sums=(
	'53dcc1dcfe161bea38c5ed2b400d10ef21a613e3a7e88d3d3edce6d27372b105'
	'5528486d640d91136276edda2075aefc06f360e6297e556051bae57b9479aeda'
)


build() {
  # Use ROS environment variables
  source /usr/share/ros-build-tools/clear-ros-env.sh
  [ -f /opt/ros/noetic/setup.bash ] && source /opt/ros/noetic/setup.bash

  # Create build directory
  [ -d ${srcdir}/build ] || mkdir ${srcdir}/build
  cd ${srcdir}/build

  # Fix Python2/Python3 conflicts
  ${srcdir}/fix-python-scripts.sh -v 3 ${srcdir}/${_dir}

  # Build project
  cmake ${srcdir}/${_dir} \
        -DCMAKE_BUILD_TYPE=Release \
        -DCATKIN_BUILD_BINARY_PACKAGE=ON \
        -DCMAKE_INSTALL_PREFIX=/opt/ros/noetic \
        -DPYTHON_EXECUTABLE=/usr/bin/python3 \
        -DPYTHON_LIBRARY=/usr/lib/libpython3.so \
        -DSETUPTOOLS_DEB_LAYOUT=OFF
  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}/" install
}
