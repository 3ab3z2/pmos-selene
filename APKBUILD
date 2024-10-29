# Reference: <https://postmarketos.org/vendorkernel>
# Kernel config based on: arch/arm64/configs/selene_defconfig

pkgname=linux-xiaomi-selene

#pkgver for fukaime: 4.14.336
#pkgver for original: 4.14.186

pkgver=4.14.336
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
	xz
    dtc
    android-tools
"

export CC="clang"
export HOSTCC="clang"

PATH="/usr/make3.81/bin:$PATH"

# Source
# original repo: "selene-kernel-original-for-pmos"
# original commit: "3887b50b55b3a057da6cf932c0426aa6989bf5d5"
# fukaime repo: "selene-kernel-fukaime"
# fukaime commit: "be4089cd5822575e6726c1a651b6d31717bb33a6"

_repository="selene-kernel-fukaime"
_commit="be4089cd5822575e6726c1a651b6d31717bb33a6"
_config="config-$_flavor.$arch"
source="
	$pkgname-$_commit.tar.gz::https://github.com/3ab3z2/$_repository/archive/$_commit.tar.gz
	$_config
    fix-check-lxdialog.patch
    gen_kheaders_cpio_fix.patch
    uaccess_h_fix.patch
    fix_dtc_overlay.patch
    use_real_mkdtb_not_python2.patch
"
builddir="$srcdir/$_repository-$_commit"
_outdir="out"

prepare() {
	default_prepare
    REPLACE_GCCH=0
	. downstreamkernel_prepare
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

sha512sums="
14423117d2659de6b2c2c9dae2ff99b2e2807af2ac102f04d617a001f31c877f503a0c75f7b06dec671383a0a18b1d828d9aa79d46c40f0205ab61086750a185  linux-xiaomi-selene-be4089cd5822575e6726c1a651b6d31717bb33a6.tar.gz
0c821d951d27498df578c195f7d99c6538ba89a6c9c912702b8f8c1c9be81d170baffa663f6dd5193c874a921883dd336d28ec4fbe173af70e0df44ca218c5d1  config-xiaomi-selene.aarch64
f748320ebe3e630b37977b6ea9f09498251cbf27368a7851b0a514853df6ad85da90cd282f62de1fbe95c551d91db82279be13611867263f4bc8aac3398aef82  fix-check-lxdialog.patch
f9b67cb6a0aebbe509b4705c187b717582838b9a5e2c0d322212e12bcb33bf921edef472858638b3b2115a47b78bfe0575d9dc0c72c9fc66d3f8458077aeb88d  gen_kheaders_cpio_fix.patch
afd4f912d3921a69059d5bd15db7991868c2c5c9da31c28763e27876731aeaec2dcbb11407648adaa61eb6967527b74212bfefc5f13b1d5b1ce93944e020cd68  uaccess_h_fix.patch
ccfb893e31635beefc891d951b461887f922c541fecd98ddc089012e7829d8c5b740ce4e03754f992b1b79d87e849db7c70bc6b6e0aeca2aee619477a9943a8e  fix_dtc_overlay.patch
ac7b3f64378c1c333c8e251936e1918096879fb6a04332cb2a197987632203c00e3d19a17f32bde1ba3f181741efc30b9086fef0aaa25cc9a799dc40890c6b21  use_real_mkdtb_not_python2.patch
"
