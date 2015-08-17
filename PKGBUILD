# Maintainer: Youpi
# Contributor: A.J. Korf <jacobkorf at gmail dot com>
# Contrubutor: Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-bumblebee
pkgver=313.30
_extramodules=extramodules-`uname -r|sed 's/\.[0-9]*\-.\-/\-/g'`
pkgrel=1
pkgdesc="NVIDIA drivers for linux.Packaged for Bumblebee"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux>=3.8' 'linux<=3.9') #"nvidia-utils-bumblebee=${pkgver}")
makedepends=('linux-headers>=3.8' 'linux-headers<=3.9')
conflicts=('nvidia-96xx' 'nvidia-173xx' 'nvidia-275xx' 'nvidia-304xx' 'dkms-nvidia')
license=('custom')
install=nvidia-bumblebee.install
options=(!strip)


if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('69c0f66c9246217a4fe4d28e95bb7bb6')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
   _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
    md5sums=('e5f147fbcdcad71472b4ddeccf259bd7')
fi

build() {
	_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
	cd "${srcdir}"
	sh ${_pkg}.run --extract-only
	cd ${_pkg}/kernel
	make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
	install -Dm644 "${srcdir}/${_pkg}/kernel/nvidia.ko" "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
	install -dm755 "${pkgdir}/usr/lib/modprobe.d"
	echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia.conf"
	sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia-bumblebee.install"
	gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
