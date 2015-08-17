# Maintainer: Redderick Shohart <s.sovetnik@gmail.com>

pkgname=qudos-zws-git
pkgver=20121119
pkgrel=1
pkgdesc="Enhanced Quake 2 engine, forked by ZwS, with yamagi's fixes, and some minor patches to prevent libjpeg and zlib compilling errors. git version"
arch=(i686)
url="https://github.com/ZwS/qudos"
license=('GPL')
# Needs sdl to compile.
depends=('git' 'libjpeg' 'libpng' 'libvorbis' 'libxxf86vm' 'mesa' 'sdl')
optdepends=('libpng-old: in case of libpng errors')
makedepends=('xf86vidmodeproto')
provides=('qudos')
conflicts=('qudos')
install=qudos.install
source=(qudos.install)
	
md5sums=('1c040c8589ce24a0abe8453f37dcf4b5')

_gamedir="/usr/share/quake2"
_libdir="/usr/lib/qudos"
_basedir="/usr/share/quake2"
_gitroot="git://github.com/ZwS/qudos.git"
_gitname="ZwS-qudos"


build() {
	msg "Connecting to GIT server...."

  if [ -d $_gitname ] ; then
    cd $_gitname && git pull origin
    msg "The local files are updated."
  else
    git clone $_gitroot $_gitname
  fi

  msg "GIT checkout done or server timeout"
  msg "Starting make..."	

  rm -rf "$srcdir/$_gitname-build"
  git clone "$srcdir/$_gitname" "$srcdir/$_gitname-build"
  
	cd "$srcdir/$_gitname-build"	
#	patch -i $startdir/gl_rmisc.patch src/ref_gl/gl_rmisc.c
	
	# Favours OpenGL over SDL - it does not lose focus when audacious/xmms starts.
	# XMMS is disabled because the Makefile blindly assumes it is installed.
	# OSS works on x86_64.
  make \
    DATADIR=$_gamedir \
    LIBDIR=$_libdir \
    LOCALBASE=/usr \
    BUILD_GLX=YES \
    BUILD_SDLGL=YES \
    BUILD_OSS_SND=YES \
    BUILD_ALSA_SND=YES \
    BUILD_QUAKE2=YES \
    BUILD_GAME=YES \
    WITH_DATADIR=YES \
    WITH_LIBDIR=YES \
    WITH_XMMS=NO \
    X11BASE=/usr/X11 \
    WITH_QMAX=YES \
    || return 1

# Binaries, libraries  & additional gl graphics

  install -D -m755 $srcdir/$_gitname-build/qudos/QuDos $pkgdir/usr/bin/QuDos || return 1
  install -D -m755 $srcdir/$_gitname-build/qudos/QuDos-ded $pkgdir/usr/bin/QuDos-ded || return 1
  
  install -D -m644 $srcdir/$_gitname/quake2/baseq2/qudos.pk3 $pkgdir/$_basedir/baseq2/qudos.pk3 || return 1
  
  install -D -m644 $srcdir/$_gitname-build/qudos/baseq2/gamei386.so $pkgdir/$_libdir/baseq2/gamei386.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/3zb2/gamei386.so $pkgdir/$_libdir/3zb2/gamei386.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/ctf/gamei386.so $pkgdir/$_libdir/ctf/gamei386.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/jabot/gamei386.so $pkgdir/$_libdir/jabot/gamei386.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/rogue/gamei386.so $pkgdir/$_libdir/rogue/gamei386.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/zaero/gamei386.so $pkgdir/$_libdir/zaero/gamei386.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/xatrix/gamei386.so $pkgdir/$_libdir/xatrix/gamei386.so || return 1

  #cp $srcdir/$_gitname-build/qudos/*.so $pkgdir/$_libdir/*.so || return 1

  install -D -m644 $srcdir/$_gitname-build/qudos/ref_q2glx.so $pkgdir/$_libdir/ref_q2glx.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/ref_q2sdlgl.so $pkgdir/$_libdir/ref_q2sdlgl.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/snd_alsa.so $pkgdir/$_libdir/snd_alsa.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/snd_oss.so $pkgdir/$_libdir/snd_alsa.so || return 1
  install -D -m644 $srcdir/$_gitname-build/qudos/snd_sdl.so $pkgdir/$_libdir/snd_sdl.so || return 1

  # Desktop entry
  install -D -m644 $srcdir/$_gitname/data/quake2.png $pkgdir/usr/share/pixmaps/quake.png || return 1
  install -D -m644 $srcdir/$_gitname/data/QuDos.desktop $pkgdir/usr/share/applications/QuDos.desktop || return 1

  # Docs
  mkdir -p $pkgdir/usr/share/doc/qudos
  cp docs/{QuDos,Ogg_readme,todo}.txt $startdir/pkg/usr/share/doc/qudos/ || return 1
}
