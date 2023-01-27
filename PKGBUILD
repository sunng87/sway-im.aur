# Maintainer: Hyeon Kim <simnalamburt@gmail.com>
# Contributor: Brett Cornwall <ainola@archlinux.org>
# Contributor: Maxim Baz <$pkgname at maximbaz dot com>
# Contributor: Alexander F. Rødseth <xyproto@archlinux.org>
# Contributor: tinywrkb <tinywrkb@gmail.com>

pkgname=sway-im
pkgver=1.8
epoch=1
pkgrel=1
pkgdesc='Tiling Wayland compositor and replacement for the i3 window manager, with input method popups v2 support'
arch=(x86_64)
url='https://swaywm.org/'
license=(MIT)
depends=(
    'cairo'
    'gdk-pixbuf2'
    'json-c'
    'pango'
    'polkit'
    'pcre'
    'swaybg'
    'ttf-font'
    'wlroots'
)
makedepends=(git meson ninja scdoc setconf wayland-protocols)
backup=(etc/sway/config)
optdepends=(
    'dmenu: Application launcher'
    'grim: Screenshot utility'
    'i3status: Status line'
    'mako: Lightweight notification daemon'
    'slurp: Select a region'
    'swayidle: Idle management daemon'
    'swaylock: Screen locker'
    'wallutils: Timed wallpapers'
    'waybar: Highly customizable bar'
    'xorg-xwayland: X11 support'
)
source=("https://github.com/swaywm/sway/releases/download/$pkgver/sway-$pkgver.tar.gz"
    #    "https://github.com/swaywm/sway/releases/download/$pkgver/sway-$pkgver.tar.gz.sig"
    "50-systemd-user.conf"
    "0001-text_input-Implement-input-method-popups.patch")
sha512sums=('0dce213939bb9b38becfac62a22cadf2dc4ed723a8fa06dcaaa3625491722e89dc92bf1734f010d0823513a45d91501c8ba6b4c71d2bf46a1e805938937bab7b'
    #    'SKIP'
    'c2b7d808f4231f318e03789015624fd4cf32b81434b15406570b4e144c0defc54e216d881447e6fd9fc18d7da608cccb61c32e0e1fab2f1fe2750acf812d3137'
    'SKIP')
# validpgpkeys=('34FF9526CFEF0E97A340E2E40FDE7BE0E88F5E48' # Simon Ser
#     '9DDA3B9FA5D58DD5392C78E652CB6609B22DA89A')          # Drew DeVault
conflicts=('sway')
provides=('sway')

prepare() {
    cd "sway-$pkgver"

    # Set the version information to 'Arch Linux' instead of 'makepkg'
    sed -i "s/branch \\\'@1@\\\'/Arch Linux/g" meson.build

    # https://github.com/swaywm/sway/pull/5890
    patch -Np1 -i ../0001-text_input-Implement-input-method-popups.patch
}

build() {
    mkdir -p build
    arch-meson build "sway-$pkgver" -D sd-bus-provider=libsystemd -D werror=false -D b_ndebug=true
    ninja -C build
}

package() {
    DESTDIR="$pkgdir" ninja -C build install
    install -Dm644 "sway-$pkgver/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    install -Dm644 50-systemd-user.conf -t "$pkgdir/etc/sway/config.d/"

    for util in autoname-workspaces.py inactive-windows-transparency.py grimshot; do
        install -Dm755 "sway-$pkgver/contrib/$util" -t \
            "$pkgdir/usr/share/sway/scripts"
    done
}

# vim: ts=2 sw=2 et
