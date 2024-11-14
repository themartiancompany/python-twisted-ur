# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Felix Yan <felixonmars@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>

_py="python"
_pyver="$( \
  "${_py}" \
    -V | \
    awk \
      '{print $2}')"
_pymajver="${_pyver%.*}"
_pyminver="${_pymajver#*.}"
_pynextver="${_pymajver%.*}.$(( \
  ${_pyminver} + 1))"
_pkg=twisted
pkgname="${_py}-${_pkg}"
pkgver=22.10.0
pkgrel=3
pkgdesc="Asynchronous networking framework written in Python"
arch=(
  'any'
)
url="https://${_pg}matrix.com/"
license=(
  'MIT'
)
depends=(
  "${_py}>=${_pymajver}"
  "${_py}<${_pynextver}"
  "${_py}-zope-interface"
  "${_py}-constantly"
  "${_py}-incremental"
  "${_py}-automat"
  "${_py}-hyperlink"
  "${_py}-attrs"
  "${_py}-typing-extensions"
)
makedepends=(
  "${_py}-setuptools"
)
optdepends=(
  "${_py}-pyopenssl: for TLS client hostname verification"
  "${_py}-service-identity: for TLS client hostname verification"
  "${_py}-idna: for TLS client hostname verification"
  "${_py}-cryptography: for using conch"
  "${_py}-pyasn1: for using conch"
  "${_py}-appdirs: for using conch"
  "${_py}-bcrypt: for using conch"
  "${_py}-h2: for http2 support"
  "${_py}-priority: for http2 support"
  "${_py}-pyserial: for serial support"
  'tk: for using tkconch'
)
checkdepends=(
  'subversion'
  'xorg-server-xvfb'
  'tk'
  'openssh'
  'git'
  'gtk3'
  "${_py}-gobject"
  "${_py}-subunit"
  "${_py}-h2"
  "${_py}-priority"
  "${_py}-cryptography"
  "${_py}-cython-test-exception-raiser"
  "${_py}-idna"
  "${_py}-bcrypt"
  "${_py}-hypothesis"
  "${_py}-pyasn1"
  "${_py}-pyhamcrest"
  "${_py}-pyopenssl"
  "${_py}-pyserial"
  "${_py}-service-identity"
)
# Conflicts with the command line tools
# used to be provided by the python2 package.
conflicts=(
  "python2-${_pkg}<=20.3.0-3"
)
source=(
  "https://github.com/twisted/twisted/archive/twisted-$pkgver.tar.gz"
)
sha512sums=(
  'cf9ed96430376d499ae9627a7d0656c05cb99bc9e9b15a8f4166355363818f090bc3c2b383ed4cf19e1e38fb569e8618d35a0ddde2a90a06f3c9a4ea769837e4'
)

build() {
  cd \
    "${_pkg}-${_pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      build
}

check() {
  export \
    LC_CTYPE=en_US.UTF-8
  # tests use the underlying function from the
  # 'python -m twisted.trial' module,
  # to prevent loading system entry points
  PYTHONPATH="$srcdir/twisted-twisted-$pkgver/build/lib" \
    xvfb-run \
      "${_py}" \
        -c \
	  'from twisted.scripts.trial import run; run()' \
	    twisted || \
      echo \
        "Tests failed"
}

package() {
  cd \
    "${_pkg}-${_pkg}-${pkgver}"
  "${_py}" \
    setup.py \
      install \
        --prefix=/usr \
	--root "${pkgdir}" \
	--optimize=1
  install \
    -Dm644 \
     LICENSE \
     -t \
     "${pkgdir}/usr/share/licenses/${pkgname}/"
}

# vim:set sw=2 sts=-1 et:
