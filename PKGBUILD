    # Maintainer: Gabrielli Gianluca <gianluca.gabrielli@autistici.org>
    pkgname=sflphone-kde-good
    pkgver=1.2.3
    pkgrel=2
    pkgdesc="SIP/IAX2 softphone, fixed the segmentation fault error from daemon, corrected the syntax error 'include' while compiling kde gui"
    url="http://www.sflphone.org"
    arch=('x86_64' 'i686')
    license=('GPL3')
    depends=('kdelibs' 'libpulse' 'libzrtpcpp' 'libyaml' 'dbus-c++' 'speex' 'libsamplerate' 'kdepimlibs')
    makedepends=('autoconf' 'cmake' 'boost' 'automoc4')
    conflicts=('sflphone')
    source=("https://projects.savoirfairelinux.com/attachments/download/6423/sflphone-${pkgver}.tar.gz")
    md5sums=('774eb07bbfc0717ad1e0090623e34616')
     
    build() {
     
    #Fix sflphoned 1.2.3 crash
    #sostituisco la riga 682 "char buf[1024];" con "char buf[65536];"
    sed -i '682s/1024/65536/' ${srcdir}/sflphone-${pkgver}/daemon/libs/pjproject-2.0.1/pjlib/src/pj/ssl_sock_ossl.c
     
    ## pjsip
    cd ${srcdir}/sflphone-${pkgver}/daemon/libs
    ./compile_pjsip.sh
     
    ## daemon
    cd "${srcdir}/sflphone-${pkgver}/daemon"
    ./autogen.sh
    ./configure --prefix="/usr" --disable-ilbc
    make
     
    #Fix KDE include error
    sed -i '7s/..\/src/../' ${srcdir}/sflphone-${pkgver}/kde/src/klib/kcfg_settings.kcfgc
     
    ## KDE client
    cd "${srcdir}/sflphone-${pkgver}/kde"
    cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr # Add -DENABLE_VIDEO=true if you want experimental video support
    make -j3
    }
     
    package() {
     
    ## daemon
      cd "${srcdir}/sflphone-${pkgver}/daemon"
      make DESTDIR="${pkgdir}" install
     
    ## KDE client
      cd "${srcdir}/sflphone-${pkgver}/kde"
      make DESTDIR="${pkgdir}" install
    }
     
    # vim:set ts=2 sw=2 et:
