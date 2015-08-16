# $Id: PKGBUILD 52694 2011-07-27 17:23:48Z schiv $
# Maintainer: Ray Rashif <schiv@archlinux.org>
# Contributor: Daniele Paolella <danielepaolella@email.it>
# Contributor: Philipp Überbacher <hollunder at gmx dot at>
# Contributor: Thomas Bahn <thomas-bahn at gmx dot net>

pkgbase=jack2-multilib
#pkgname=('jack2-multilib' 'jack2-dbus-multilib')
pkgname=jack2-multilib
_tarname=jack
pkgver=1.9.7
pkgrel=2
arch=('i686' 'x86_64')
provides=('jack' 'jack2' 'lib32-jack')
url="http://jackaudio.org/"
license=('GPL')
makedepends=('python2' 'doxygen' 'libffado' 'celt' 'expat' 'lib32-expat'
             'libsamplerate' 'dbus-core' 'gcc-multilib')
source=("http://www.grame.fr/~letz/$_tarname-$pkgver.tar.bz2")
md5sums=('9759670feecbd43eeccf1c0f743ec199')

_pyfix() {
  sed -i 's:bin/env python:bin/env python2:' "$pkgdir/usr/bin/jack_control"
}

_wafconf() {
  python2 waf configure --prefix=/usr --alsa --firewire --doxygen $@
}

build() {
  cd "$srcdir"

  # fix doxygen building
  sed -i 's:build/default/html:html:' $_tarname-$pkgver/wscript

  # we're going to do 2 different builds
  cp -r $_tarname-$pkgver $_tarname-dbus-$pkgver

  # mixed dbus/classic build
  cd $_tarname-$pkgver
  msg2 "Running Mixed D-Bus/Classic build"
  _wafconf --classic --dbus --mixed
  python2 waf build $MAKEFLAGS

  # dbus-ONLY build
  #cd ../$_tarname-dbus-$pkgver
  #msg2 "Running D-Bus-only build"
  #_wafconf --dbus --mixed
  #python2 waf build $MAKEFLAGS
}

#package_jack2-multilib() {
package() {
  pkgdesc="The next-generation JACK with SMP support (64 and 32 bit package)"
  depends=('libsamplerate')
  optdepends=('libffado: FireWire support'
              'celt: NetJACK2 driver'
              'dbus-core: jackdbus'
              'python2: jack_control')
  conflicts=('jack')
  provides=('jack' 'jack-audio-connection-kit' 'jackdbus'
            'jack-audio-connection-kit-mp' 'jackmp' 'jackdmp')

  cd "$srcdir/$_tarname-$pkgver"

  python2 waf install --destdir="$pkgdir"

  # fix for major python transition
  _pyfix
}

#package_jack2-dbus-multilib() {
#  pkgdesc="The next-generation JACK with SMP support (for D-BUS interaction only) (64 and 32 bit package)"
#  depends=('libsamplerate' 'dbus-core')
#  optdepends=('libffado: FireWire support'
#              'celt: NetJACK2 driver'
#              'python2: jack_control')
#  conflicts=('jack' 'jack2')
#  provides=('jack' 'jack2' 'jack-audio-connection-kit' 'jackdbus'
#            'jack-audio-connection-kit-mp' 'jackmp' 'jackdmp')
#
#  cd "$srcdir/$_tarname-dbus-$pkgver"
#
#  python2 waf install --destdir="$pkgdir"
#
#  _pyfix
#}

# vim:set ts=2 sw=2 et:
