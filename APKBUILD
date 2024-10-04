# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm64/configs/(CHANGEME!)

pkgname=linux-xiaomi-selene
pkgver=4.14.186
pkgrel=0
pkgdesc="Xiaomi Redmi 10 2022 kernel fork"
arch="aarch64"
_carch="arm64"
_flavor="xiaomi-selene"
url="https://kernel.org"
license="GPL-2.0-only"
options="!strip !check !tracedeps pmb:cross-native"
makedepends="
	clang
	bash
	bc
	bison
	devicepkg-dev
	findutils
	flex
	openssl-dev
	perl
	make3.81
"
export CC="clang"
export HOSTCC="clang"

subpackages="$pkgname-cpio"

PATH="/usr/make3.81/bin:$PATH"

# Source
_repository="Xiaomi_Kernel_OpenSource"
_commit="6a5cdd2759876185ce2381313fef9752d7cb56a4"
#_cpio="cpio-2.12"
#original kernel
#3887b50b55b3a057da6cf932c0426aa6989bf5d5
#update kernel
#6a5cdd2759876185ce2381313fef9752d7cb56a4
#notpatchedcpio-> https://ftp.gnu.org/gnu/cpio/cpio-2.12.tar.gz
_config="config-$_flavor.$arch"
source="
	$pkgname-$_commit.tar.gz::https://github.com/MiCode/$_repository/archive/$_commit.tar.gz
	cpio-2.12.tar.gz::https://git.savannah.gnu.org/cgit/cpio.git/snapshot/cpio-641d3f489cf6238bb916368d4ba0d9325a235afb.tar.gz
	$_config
	fix-check-lxconfig.patch
	mt_get_uartlog_status.patch
	use_right_as.patch
	fix-timer-typo.patch
	makefile-fixes.patch
"
builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
	default_prepare
	REPLACE_GCCH=0
	. downstreamkernel_prepare
	# Build cpio-2.12
    if [ ! -d "$srcdir/cpio-2.12" ]; then
        tar -xvf "$srcdir/cpio-2.12.tar.gz" -C "$srcdir"
    fi
    cd "$srcdir/cpio-2.12"
    ./configure --prefix=/usr
    make
    make install DESTDIR="$srcdir/cpio-2.12"
}

build() {
	unset LDFLAGS
	make O="$_outdir" ARCH="$_carch" CC="${CC:-gcc}" \
		KBUILD_BUILD_VERSION="$((pkgrel + 1 ))-postmarketOS"
}

package() {
	downstreamkernel_package "$builddir" "$pkgdir" "$_carch" \
		"$_flavor" "$_outdir"

	make dtbs_install O="$_outdir" ARCH="$_carch" \
		INSTALL_DTBS_PATH="$pkgdir"/boot/dtbs
}

package_cpio() {
    mkdir -p "$subpkgdir"/usr/bin
    cp "$srcdir/cpio-2.12"/usr/bin/cpio "$subpkgdir"/usr/bin/
    cp "$srcdir/cpio-2.12"/usr/share/man/man1/cpio.1 "$subpkgdir"/usr/share/man/man1/
}

sha512sums="
53ebdc069723b73371eac677a6ef4deda235b4de3ab08b199f58a25e00bc03292aa0714da0c7532a055f0816740897550a7f7bf512c4e3fe072270a35ab5af6a  linux-xiaomi-selene-6a5cdd2759876185ce2381313fef9752d7cb56a4.tar.gz
640a6980273b699dba147e7b656440d3bd09c1c3ac71650f218ca1e4b4309c04b391ff2a8569e12d7a95827dd2781ab369542de20cc3c26dab3431f2efbdc905  cpio-2.12.tar.gz
b8eb47bb8c31ffb2f5e3bbbf1bc1dea5c4bb765f19ddb2e5e5c3c2570dfe3557761202e495b729412199ee1767a2fb0e191c28be36d57de5636cdb20c877285c  config-xiaomi-selene.aarch64
f748320ebe3e630b37977b6ea9f09498251cbf27368a7851b0a514853df6ad85da90cd282f62de1fbe95c551d91db82279be13611867263f4bc8aac3398aef82  fix-check-lxconfig.patch
fe1c761ced52a79e60428b07d277e63703711d6628e3790aa141c826047cbbd55342c9cf13641702a12f196f76574c0bbb1158accd4271cc8c68fb4883c89fe6  mt_get_uartlog_status.patch
8f06232fac6885901a777c22b52fa06f988a9e5f212cdc031cbc14e0b4fc5a5aef3b58e60228de76e9b10c109e63d332e9304d3d7aff4c92b10592e8ea44fe7a  use_right_as.patch
66f0dc0c54ef894b358b17c152e7811f14a71352085cdde8237671339c54ee98239ab390d4a7697faa8f96f1b9687f1109c760931eccfe67e58ca418239ebb66  fix-timer-typo.patch
df2fbd7851c5d6bcc2c79bb93b75133e948b1d70e6ed5259831c8a5b4332e763485f12f2ee6dacc03537fd39bc17e333157405d5fa5e79f827b18468ae357712  makefile-fixes.patch
"
