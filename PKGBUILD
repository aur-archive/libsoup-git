# Maintainer: Yosef Or Boczko <yosefor3@walla.com>

pkgbase=libsoup-git
pkgname=libsoup-git
true && pkgname=(libsoup-git libsoup-gnome-git)
pkgver=20130320
pkgrel=1
_realver=2.41.92
pkgdesc="Gnome HTTP Library"
arch=('i686' 'x86_64')
url="http://live.gnome.org/LibSoup"
license=('LGPL')
makedepends=('autogen' 'intltool' 'pkgconfig' 'gtk-doc' 'git'
  'automake' 'gnome-common' 'glib2>=2.33.1' 'libxml2'
  'sqlite' 'libgnome-keyring' 'intltool' 'gobject-introspection'
  'glib-networking')
options=('!libtool' '!emptydirs')

_gitroot=http://git.gnome.org/browse/libsoup
_gitname=libsoup

build() {
  cd "${srcdir}"
  msg "Connecting to GIT server...."
  if [ -d ${_gitname} ] ; then
    cd ${_gitname} && git pull origin
    msg "The local files are updated."
  else
    git clone ${_gitroot} ${_gitname}
  fi

  msg "GIT checkout done or server timeout"
  rm -rf "${srcdir}/${_gitname}-build"
  git clone "${srcdir}/${_gitname}" "${srcdir}/${_gitname}-build"
  cd "${srcdir}/${_gitname}-build"

  msg "Starting make..."
  cd "${srcdir}/${_gitname}-build"
  
  ./autogen.sh --prefix=/usr --sysconfdir=/etc \
    --localstatedir=/var --disable-static
    
  cd tests
  cat > Makefile << EOF
all:
install:
EOF
  
  cd ..
  
  make
}

package_libsoup-git() {
  provides=("libsoup=${_realver}")
  conflicts=('libsoup')
  depends=('glib2>=2.33.1' 'libxml2' 'glib-networking')

  cd "${srcdir}/${_gitname}-build"
  make DESTDIR="$pkgdir" install

  rm -f "$pkgdir"/usr/lib/libsoup-gnome-2.4.*
  rm -f "$pkgdir/usr/lib/pkgconfig/libsoup-gnome-2.4.pc"
  rm -rf "$pkgdir/usr/include/libsoup-gnome-2.4"
  rm -f "$pkgdir/usr/lib/girepository-1.0/SoupGNOME-2.4.typelib"
}

package_libsoup-gnome-git() {
  provides=("libsoup-gnome=${_realver}")
  conflicts=('libsoup-gnome')
  pkgdesc="GNOME HTTP Library - GNOME libraries"
  depends=("libsoup=${_realver}" 'libgnome-keyring' 'sqlite')

  cd "${srcdir}/${_gitname}-build"
  make DESTDIR="$pkgdir" install

  rm -f "$pkgdir"/usr/lib/libsoup-2.4.*
  rm -f "$pkgdir/usr/lib/pkgconfig/libsoup-2.4.pc"
  rm -rf "$pkgdir/usr/include/libsoup-2.4"
  rm -rf "$pkgdir/usr/share"
  rm -f "$pkgdir/usr/lib/girepository-1.0/Soup-2.4.typelib"
}
