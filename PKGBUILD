# Maintainer: Krutonium <krutonium@krutonium.ga>
pkgname=elgato-gchd
pkgver=r147.e5bc6b9
pkgrel=1
pkgdesc="A userspace driver for the Elgato Gamecapture HD in Linux"
arch=('x86_64')
url='https://github.com/tolga9009/elgato-gchd'
license=('MIT')
depends=('libusb>=1.0.20')
makedepends=('cmake' 'p7zip' 'clang' 'git')
optdepends=('obs-studio: For use in streams and recordings')
source=('git+https://github.com/tolga9009/elgato-gchd.git' 'https://edge.elgato.com/egc/macos/egcm/2.11.8/final/Game_Capture_HD_2.11.8.zip')
md5sums=('SKIP' '9cc176d18d693d65debccd5d85506e59')

pkgver() {
	cd "$pkgname"
	printf 'r%s.%s' "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}


build() {
	mkdir -p build
	cd build
	cmake ../elgato-gchd/
	make
}

package() {
	install -D "./build/src/gchd" "$pkgdir/opt/$pkgname/gchd"
	echo "#!/bin/sh" > gchd.sh
	echo "cd /opt/$pkgname && exec ./gchd $@" >> gchd.sh
	install -Dm755 gchd.sh "$pkgdir/usr/bin/gchd"
	7z e "$srcdir/Game_Capture_HD_2.11.8.zip" 'Game Capture HD.app/Contents/Resources/Firmware/Beddo/'{mb86m01_assp_nsec_idle.bin,mb86m01_assp_nsec_enc_h.bin} -o"./firmware/"
	install -Dm775 "./firmware/mb86m01_assp_nsec_idle.bin" "$pkgdir/usr/lib/firmware/gchd/mb86m01_assp_nsec_idle.bin"
	install -Dm775 "./firmware/mb86m01_assp_nsec_enc_h.bin" "$pkgdir/usr/lib/firmware/gchd/mb86m01_assp_nsec_enc_h.bin"	
	install -Dm755 "./elgato-gchd/udev-rules/55-elgato-game-capture.rules" "$pkgdir/etc/udev/rules.d/55-elgato-game-capture.rules"
	
	echo "Either use gchd as sudo, or"
	echo "sudo usermod <username> -aG video"
	echo "then reboot"
}
