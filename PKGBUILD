# Maintainer: ThatOneCalculator <kainoa@t1c.dev>

_pkgname="hyprland"
pkgname="${_pkgname}"
pkgver="0.6.2beta"
pkgrel=1
pkgdesc="A dynamic tiling Wayland compositor based on wlroots that doesn't sacrifice on its looks."
arch=(any)
url="https://github.com/vaxerski/Hyprland"
license=('BSD')
depends=(
	libxcb
	xcb-proto
	xcb-util
	xcb-util-keysyms
	libxfixes
	libx11
	libxcomposite
	xorg-xinput
	libxrender
	pixman
	wayland-protocols
	cairo
	pango
	polkit
	glslang
	libinput
	libxcb
	libxkbcommon
	opengl-driver
	pixman
	wayland
	xcb-util-errors
	xcb-util-renderutil
	xcb-util-wm
	seatd
	vulkan-icd-loader
	vulkan-validation-layers
	xorg-xwayland)
makedepends=(
	git
	cmake
	ninja
	gcc
	gdb
	meson
	vulkan-headers
	wayland-protocols
	xorgproto)
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/vaxerski/hyprland/archive/v${pkgver}.tar.gz")
sha256sums=('f5c3969171d579ac33237e71b94bfbdd110c5169c988c6f5ef10a3f4408b5750')
conflicts=("${_pkgname}")
provides=(hyprland)
options=(!makeflags !buildflags !strip)

build() {
	cd "${srcdir}/${_pkgname}"
	git submodule update --init
	make fixwlr
	cd "./subprojects/wlroots/" && meson build/ --prefix="${srcdir}/tmpwlr" --buildtype=release && ninja -C build/ && mkdir -p "${srcdir}/tmpwlr" && ninja -C build/ install && cd ../../
	make protocols
    make release
	cd ./hyprctl && make all && cd ..
}

package() {
	cd "$srcdir/Hyprland-$pkgver"
	mkdir -p "${pkgdir}/usr/share/wayland-sessions"
	mkdir -p "${pkgdir}/usr/share/hyprland"
	install -Dm755 build/Hyprland -t "${pkgdir}/usr/bin"
	install -Dm755 hyprctl/hyprctl -t "${pkgdir}/usr/bin"
	install -Dm644 assets/*.png -t "${pkgdir}/usr/share/hyprland"
	install -Dm644 example/hyprland.desktop -t "${pkgdir}/usr/share/wayland-sessions"
	install -Dm644 example/hyprland.conf -t "${pkgdir}/usr/share/hyprland"
	install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${_pkgname}"
	install -Dm755 ../tmpwlr/lib/libwlroots.so.11032 -t "${pkgdir}/usr/lib"
}
