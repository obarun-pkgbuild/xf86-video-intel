# Maintainer: Eric Vidal <eric@obarun.org>
# based on the original https://git.archlinux.org/svntogit/packages.git/log/trunk?h=packages/xf86-video-intel
# 						Maintainer: AndyRTR <andyrtr@archlinux.org>
#						Maintainer: Jan de Groot <jgc@archlinux.org>

pkgname=xf86-video-intel
_commit=d7dfab62a9853b44bbcd67dac08391d8e5114c97 # master
pkgver=2.99.917+829+gd7dfab62
pkgrel=2
epoch=1
arch=(x86_64)
url="https://01.org/linuxgraphics"
license=('custom')
install=$pkgname.install
pkgdesc="X.org Intel i810/i830/i915/945G/G965+ video drivers"
depends=('mesa' 'libxvmc' 'pixman' 'xcb-util>=0.3.9')
makedepends=('xorg-server-devel' 'X-ABI-VIDEODRV_VERSION=23' 'libx11' 'libxrender'
             # additional deps for intel-virtual-output
             'libxrandr' 'libxinerama' 'libxcursor' 'libxtst' 'libxss'
             # additional for git snapshot
             'git')
optdepends=('libxrandr: for intel-virtual-output'
            'libxinerama: for intel-virtual-output'
            'libxcursor: for intel-virtual-output'
            'libxtst: for intel-virtual-output'
            'libxss: for intel-virtual-output')
replaces=('xf86-video-intel-uxa' 'xf86-video-intel-sna')
provides=('xf86-video-intel-uxa' 'xf86-video-intel-sna')
conflicts=('xorg-server<1.19' 'X-ABI-VIDEODRV_VERSION<23' 'X-ABI-VIDEODRV_VERSION>=24'
           'xf86-video-intel-sna' 'xf86-video-intel-uxa' 'xf86-video-i810' 'xf86-video-intel-legacy')
groups=('xorg-drivers')
source=("git+https://anongit.freedesktop.org/git/xorg/driver/xf86-video-intel#commit=$_commit")
sha256sums=('SKIP')
validpgpkeys=('6DD4217456569BA711566AC7F06E8FDE7B45DAAC') # Eric Vidal


pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname
  NOCONFIGURE=1 ./autogen.sh
}

build() {
  cd $pkgname
  
  # Since pacman 5.0.2-2, hardened flags are now enabled in makepkg.conf
  # With them, module fail to load with undefined symbol.
  # See https://bugs.archlinux.org/task/55102 / https://bugs.archlinux.org/task/54845
  export CFLAGS=${CFLAGS/-fno-plt}
  export CXXFLAGS=${CXXFLAGS/-fno-plt}
  export LDFLAGS=${LDFLAGS/,-z,now}

  ./configure --prefix=/usr \
    --libexecdir=/usr/lib \
    --with-default-dri=3 \
    --enable-udev
  make
}

check() {
  cd $pkgname
  make check
}

package() {
  cd $pkgname

  make DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/licenses/${pkgname}"
  install -m644 COPYING "${pkgdir}/usr/share/licenses/${pkgname}/"
}
