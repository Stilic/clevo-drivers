# Maintainer: Nick Dowsett <nick42d AT gmail DOT com>
pkgname=clevo-drivers-dkms-git
_pkgname=clevo-drivers
pkgver=4.6.3
pkgrel=1
pkgdesc="Kernel module drivers for keyboard, keyboard backlight & general hardware I/O using the SysFS interface for Clevo hardware."
url="https://gitlab.com/evorster/clevo-drivers"
license=("Other")
arch=('x86_64')
depends=('dkms')
makedepends=('git')
optdepends=('linux-headers: build modules against Arch kernel'
            'linux-lts-headers: build modules against LTS kernel'
            'linux-zen-headers: build modules against ZEN kernel'
            'linux-hardened-headers: build modules against the HARDENED kernel')
# tuxedo-keyboard-ite = ite_8291, ite_8291_lb, ite_8297 and ite_829x
provides=('tuxedo-drivers-dkms'
	  'tuxedo-keyboard'
          'tuxedo-keyboard-ite'
          'tuxedo-io'
          'clevo-wmi'
          'clevo-acpi'
          'uniwill-wmi'
          'ite_8291'
          'ite_8291_lb'
          'ite_8297'
          'ite_829x')
conflicts=('tuxedo-drivers-dkms' 'tuxedo-keyboard-dkms' 'tuxedo-keyboard-ite-dkms')
#backup=(etc/modprobe.d/tuxedo_keyboard.conf)
source=(git+https://gitlab.com/tuxedocomputers/development/packages/tuxedo-drivers.git#tag=v${pkgver} patch.diff tuxedo_io.conf dkms.conf)
sha256sums=('64becfa3d5b39c6b6f5b0c39f4cee7bcaf3f76d84a6f955635698351ac4fd01b'
            'd6bef54bcf39e5aa24b3f1148c4ffc65dd054a23ca3af44787a7d1010169b6b6'
            'd94d305bfd2767ad047bc25cc5ce986e76804e7376c3dd4d8e500ebe2c7bef3c'
            '4c83b8508698c49246fedf31dbe329ceff5132707afb4fa2df4ccc6c9e1e9db8')

prepare(){
	echo "$(ls)"
	cd ${srcdir}/tuxedo-drivers
	echo "Current directory is: $(pwd)"
	pkgver=$(git tag | tail -1 | tr -d "v")
	echo "pkgver is "${pkgver}
	patch -Np1 -i ../patch.diff
}

package() {
	echo "Package step"
	mkdir -p "${pkgdir}/usr/src/${_pkgname}-${pkgver}"
	ln -s ${srcdir}/tuxedo-drivers ${srcdir}/${_pkgname}-${pkgver}
	sed -i "s/#MODULE_VERSION#/${pkgver}/" dkms.conf
	install -Dm644 dkms.conf -t "$pkgdir/usr/src/${_pkgname%}-$pkgver/"
	install -Dm644 "${_pkgname%}-$pkgver"/Makefile -t "$pkgdir/usr/src/${_pkgname%}-$pkgver/"
	install -Dm644 "${_pkgname%}-$pkgver"/tuxedo_keyboard.conf -t "$pkgdir/usr/lib/modprobe.d/"
	install -Dm644 "$srcdir/tuxedo_io.conf" -t "$pkgdir/usr/lib/modules-load.d/"
	#cp -avr "${_pkgname%}-$pkgver"/* "$pkgdir/usr/src/${_pkgname%}-$pkgver/"
	cp -avr "${_pkgname%}-$pkgver"/src/* "$pkgdir/usr/src/${_pkgname%}-$pkgver/"
}

