# copied and modified from https://raw.githubusercontent.com/archlinux/svntogit-community/packages/nnn/trunk/PKGBUILD
# check original for better updated version.
# Maintainer: Felix Yan <felixonmars@archlinux.org
# Maintainer: Alexander Epaneshnikov <alex19ep@archlinux.org>
# Contributor: Maxim Baz <$pkgname at maximbaz dot com>
# Contributor: Pablo Arias <pabloariasal@gmail.com>
# Contributor: John Jenkins <twodopeshaggy@gmail.com>

pkgname=nnn-navn
pkgver=4.7.r5.g70aabc14
pkgrel=1
pkgdesc="The fastest terminal file manager ever written."
arch=('x86_64')
depends=('bash' 'sed')
optdepends=(
    'atool: for more archive formats'
    'libarchive: for more archive formats'
    'zip: for zip archive format'
    'unzip: for zip archive format'
    'trash-cli: to trash files'
    'sshfs: mount remotes'
    'rclone: mount remotes'
    'fuse2: unmount remotes'
    'xdg-utils: desktop opener'
)
url="https://github.com/navneetankur/nnn"
license=('BSD')
source=("git+${url}.git")
md5sums=('SKIP')
makedepends=(git)
provides=(nnn)
conflicts=(nnn)

pkgver() {
	cd nnn
	git describe --long --tags | sed -r 's/([^-]*-g)/r\1/;s/-/./g;s/v//g'
}

prepare() {
    sed -i 's/install: all/install:/' "nnn/Makefile"
}

build() {
    cd "nnn"
	if [ "$CARCH" = "aarch64" -o "$CARCH" = "armv7l" ]; then
		make O_NORL=1 O_NOBATCH=1 O_NOFIFO=1 O_NOSSN=1 O_NOUG=1 O_NOLC=1 O_NOX11=1 O_QSORT=1
	else
		make O_NERD=1
	fi
}

package() {
    cd "nnn"
    make DESTDIR="${pkgdir}" PREFIX=/usr install
    make DESTDIR="${pkgdir}" PREFIX=/usr install-desktop

    install -Dm644 misc/auto-completion/fish/nnn.fish "${pkgdir}/usr/share/fish/vendor_completions.d/nnn.fish"
    install -Dm644 misc/auto-completion/bash/nnn-completion.bash "${pkgdir}/usr/share/bash-completion/completions/nnn"
    install -Dm644 misc/auto-completion/zsh/_nnn "${pkgdir}/usr/share/zsh/site-functions/_nnn"

    install -Dm644 -t "${pkgdir}/usr/share/nnn/quitcd/" misc/quitcd/*

    cp -a plugins "${pkgdir}/usr/share/nnn/plugins/"

    install -Dm644 -t "${pkgdir}/usr/share/licenses/${pkgname}/" LICENSE
}
