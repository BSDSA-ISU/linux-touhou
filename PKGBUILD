# Maintainer: Ali Ali <AlieAlii@goo.su>
pkgname="linux-koishi"
pkgver=6.12.102
pkgrel=1
pkgdesc="Linux kernel made with koishi power"
arch=('x86_64')
url="https://kernel.org/"
license=('GPL-2.0-only')
depends=('glibc')
replaces=('linux-hoshino' '$pkgname')
makedepends=('coreutils' 'base-devel' 'bc' 'kmod'  'libelf' 'libcap' 'ninja' 'python' 'pahole' 'pacman')
optdepends=('coreutils: simple Linux tools functionality.'
    "sway: best simple waylander experience"
    "mkinitcpio: Modular initramfs image creation utility"
)
_remote="https://kernel.googlesource.com/pub/scm/linux/kernel/git/stable/linux"
install=linux.install
provides=("koishi" "modules" 'linux-koishi')
conflicts=("linux-kyouko" "$pkgname")

prepare() {
  git clone \
    --depth 1 \
    --single-branch \
    --branch linux-6.12.y \
    $_remote \
    "$srcdir" || return 0
  cd "$srcdir"
  echo "Using this kernel's default config"
  zcat /proc/config.gz > .config
  echo "Finalize or configure it..."
  make menuconfig
}

build() {
    mkdir -p "$srcdir/usr"
    cd "$srcdir"
    make headers_install INSTALL_HDR_PATH="$srcdir/usr"
    make tar-pkg
    mkdir -p etc/mkinitcpio.d
}

package() {
    tar -xvf $srcdir/*.tar -C "$pkgdir"
    mv -v $pkgdir/boot/vmlinux* $pkgdir/boot/vmlinux-koishi || return 0
    mv -v $pkgdir/boot/System.* $pkgdir/boot/System.map || return 0
    mv -v $pkgdir/boot/config* $pkgdir/boot/config-koishi || return 0
    mv -v $pkgdir/boot/vmlinuz* $pkgdir/boot/vmlinuz-koishi || return 0
    mkdir -p $pkgdir/usr
    mv -v $pkgdir/lib $pkgdir/usr/lib || return 0
    mkdir -p "$pkgdir/etc/mkinitcpio.d/" || return 0
    cp -rvf ../package.preset "$pkgdir/etc/mkinitcpio.d/linux-koishi.preset"
}
