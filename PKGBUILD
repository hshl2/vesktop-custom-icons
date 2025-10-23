# Maintainer: slip <aur.57652 AT 8shield DOT net>

_pkgname=vesktop
pkgname=vesktop-custom-icons-git
pkgdesc="A standalone Electron-based Discord app with Vencord & improved Linux support built with original discord icons"
pkgver=1.6.0.r9.ga242d5d
pkgrel=1

arch=("x86_64" "aarch64")
url="https://github.com/Vencord/Vesktop"
license=('GPL-3.0-only')

depends=('alsa-lib' 'gtk3' 'nss')
makedepends=('git' 'pnpm' 'npm' 'imagemagick' 'optipng')
optdepends=(
  'libnotify: Notifications'
  'xdg-utils: Open links, files, etc'
)

provides=("vesktop")
conflicts=('vesktop' 'vesktop-git')

: "${color:=default}"

source=(
  "$_pkgname::git+$url.git"
  "vesktop.desktop"
  "vesktop.sh"
  "resize.sh"
  "colors.conf"
  "https://raw.githubusercontent.com/username/repository/branch/path/to/loader.gif"
  "https://raw.githubusercontent.com/username/repository/branch/path/to/source-icon.svg"
)

sha256sums=('SKIP'
            '455c00b862aa0a7e18ca8e23d65d5c5ee4506cdfb15f1bf6f622cce39827de46'
            '506c246328af639d6f6a3e52215c7b34af2a6df11d195de6f57a8bbee750cce9'
            '10140556459d948a68a6cd4c09711774c674ba2c4c9e9342fe90389804e26aba'
            '83f357f337836a9f99d915fb8607a28d131be5af9310000def13655d83784427'
            'eafb4260de50e0708f992b3452244d4285374167c142909bed534da556164b7a'
            '5aaabbf078189547b0d19f2de1a117513dd090681b85b98dcca24ce624647c8c')

pkgver() {
  cd "$_pkgname"
  git describe --long --tags --abbrev=7 --exclude='*[a-zA-Z][a-zA-Z]*' \
    | sed -E 's/^[^0-9]*//;s/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
    cd "$srcdir"

    echo ">> Generating icons using color scheme: ${color:-default}"
    bash resize.sh "$color"
    echo "Icons generated in pack"

    cd "$_pkgname"

    BUILD_STATIC_DIR="build/static"
    mkdir -p "$BUILD_STATIC_DIR"

    cp -v "$srcdir/pack/static/vesktop.png" "$srcdir/$_pkgname/static/icon.png"
    cp -v "$srcdir/pack/static/icon.ico" "$srcdir/$_pkgname/static/icon.ico"
    cp -v "$srcdir/pack/static/shiggy.gif" "$srcdir/$_pkgname/static/shiggy.gif"

    echo "Icons injected into build/static/"
}

build() {
    cd "$srcdir/$_pkgname"

    pnpm i --frozen-lockfile
    pnpm package:dir
}

package() {
    cd "$srcdir/$_pkgname"

    install -d "$pkgdir/usr/lib/$_pkgname"
    install -d "$pkgdir/usr/bin"

    cp -R "dist/linux-unpacked/." "$pkgdir/usr/lib/$_pkgname"

    install -Dm644 "../vesktop.desktop" "$pkgdir/usr/share/applications/vesktop.desktop"
    install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"

    for _icons in 1024 512 256 128 64 48 32 16; do
        install -Dm644 "../pack/icons/hicolor/${_icons}x${_icons}/apps/vesktop.png" \
            "$pkgdir/usr/share/icons/hicolor/${_icons}x${_icons}/apps/vesktop.png"
    done

    install -Dm644 "../pack/static/vesktop.png" "$pkgdir/usr/share/icons/hicolor/1080x1080/apps/vesktop.png"
    install -Dm644 "../pack/static/icon.ico" "$pkgdir/usr/share/icons/hicolor/1080x1080/apps/icon.ico"
    install -Dm644 "../pack/static/shiggy.gif" "$pkgdir/usr/share/icons/hicolor/1080x1080/apps/shiggy.gif"

    install -Dm755 "../vesktop.sh" "$pkgdir/usr/bin/$_pkgname"
}

