
# Contributor: chenxing <cxcxcxcx AT gmail DOT com>
# Contributor: Michael Burkhard <Michael DOT Burkhard AT web DOT de>
# Maintainer: alexmo82 <25396682 AT live DOT it>

pkgname=freefilesync
pkgver=9.7
pkgrel=2
pkgdesc="Backup software to synchronize files and folders"
arch=('i686' 'x86_64')
url="http://www.freefilesync.org/"
license=('GPLv3')
depends=(wxgtk webkit2gtk boost-libs)
makedepends=(boost)
source=(
	"FreeFileSync_${pkgver}_Source.zip::https://www.freefilesync.org/download_redirect.php?file=FreeFileSync_${pkgver}_Source.zip"		#ffs
	FreeFileSync.desktop
	ffsicon.png
	RealTimeSync.desktop
	rtsicon.png
	)

sha256sums=(
	 'fe26cd2664501da266582a3bb744ff0267fc7364004305b44e0624d8dfb92246'	#ffs source
	 'b381bb9dbda25c3c08a67f18072a2761abe34339ddf3318e1758eb7c349f1a3b'	#FreeFileSync.desktop
	 '31df3fa1f1310de14bbd379f891d4f8ed2df5b0d68913eb52c88b3be682933fb'	#ffsicon.png
	 '1502efdbf1638856a18ab9916e0431bf6a53471792cb2daa380345bac33f67c4'	#RealTimeSync.desktop
	 'f28042587dbe99cf5d6bef2c1be4b026488e418e4ba8332b3016d246b7053a4e'	#rtsicon.png
	 )
	 
DLAGENTS=('https::/usr/bin/curl -fLC - --retry 3 --retry-delay 3 -A Mozilla -o %o %u')

prepare() {
# wxgtk < 3.1.0
    sed -i 's/m_listBoxHistory->GetTopItem()/0/g'		FreeFileSync/Source/ui/main_dlg.cpp

# gcc 6.3.1
    sed -i 's!static_assert!//static_assert!'			zen/scope_guard.h

# warn_static(string)
    sed -i 's!-O3 -DN!-D"warn_static(arg)= " -O3 -DN!'		FreeFileSync/Source/Makefile
    sed -i 's!-O3 -DN!-D"warn_static(arg)= " -O3 -DN!'		FreeFileSync/Source/RealTimeSync/Makefile

# linker error
    sed -i 's#inline##g' FreeFileSync/Source/ui/version_check_impl.h

# install error
    cp ${srcdir}/Changelog.txt ${srcdir}/FreeFileSync/Build
}

build() {
### speed up compile on multithread machines
    MAKEFLAGS="-j$(nproc)"

### just in case of compile errors
    VER=`g++ -dumpversion`
    MAC=`g++ -dumpmachine`
    echo "compiler g++ $VER $MAC"

### FFS
    cd ${srcdir}/FreeFileSync/Source
    make launchpad

### RTS
    cd RealTimeSync
    make launchpad
}

package() {
    cd ${srcdir}/FreeFileSync/Source
    make DESTDIR=${pkgdir} install

    cd RealTimeSync
    make DESTDIR=${pkgdir} install

    cd ${srcdir}
    install -Dm644 FreeFileSync.desktop $pkgdir/usr/share/applications/FreeFileSync.desktop
    install -Dm644 ffsicon.png $pkgdir/usr/share/pixmaps/ffsicon.png
    install -Dm644 RealTimeSync.desktop $pkgdir/usr/share/applications/RealTimeSync.desktop
    install -Dm644 rtsicon.png $pkgdir/usr/share/pixmaps/rtsicon.png
}
