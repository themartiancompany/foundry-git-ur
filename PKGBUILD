# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Oliver Nordbjerg <hi@notbjerg.me>

_os="$( \
  uname \
    -o)"
_git=false
_pkg=foundry
_pkgname="${_pkg}"
pkgname="${_pkgname}-git"
pkgver=r2015.925626516
pkgrel=1
_pkgdesc=(
  "A blazing fast, portable and modular toolkit"
  "for Ethereum application development written in Rust."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'aarch64'
  'arm'
  'mips'
  'i686'
  'powerpc'
  'armv7l'
)
url="https://getfoundry.sh"
license=(
  'APACHE'
  'MIT'
)
depends=(
  libusb
)
makedepends=(
  'git'
  'cargo'
)
provides=(
  forge
  cast
  anvil
  chisel
)
_http="https://github.com"
_ns="${_pkgname}-rs"
_url="${_http}/${_ns}/${_pkgname}"
_branch="master"
[[ "${_git}" == true ]] && \
  makedepends+=(
    git
  ) && \
  source+=(
    "${_pkgname}-${_branch}::git+${_url}#branch=${_branch}"
  )
[[ "${_git}" == false ]] && \
  makedepends+=(
    curl
    jq
  ) && \
  source+=(
    "${_pkgname}-${_branch}.tar.gz::${_url}/archive/refs/heads/${_branch}.tar.gz"
  )
md5sums=(
  'SKIP'
)

pkgver() {
  cd \
    "${_pkgname}-${_branch}"
  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

prepare() {
  local \
    _target
  _target="${CARCH}-unknown-linux-gnu"
  [[ "${_os}" == "Android" ]] && \
    _target="${CARCH}-linux-androideabi"
  cd \
    "${_pkgname}-${_branch}"
  cargo \
    fetch \
      --locked \
      --target \
      "${_target}"
}

build() {
  cd \
    "${_pkgname}-${_branch}"
  export \
    RUSTUP_TOOLCHAIN=stable
  export \
    CARGO_TARGET_DIR=target
  cargo \
    build \
      --frozen \
      --release \
      --all-features
}

package() {
  cd \
    "${_pkgname}-${_branch}"
  find \
    target/release \
    -maxdepth \
      1 \
    -executable \
    -type \
      f \
    -exec \
      install \
        -Dm0755 \
	-t \
	"${pkgdir}/usr/bin/" \
	{} \
	+
  # TODO completions
  # TODO doc
  install \
    -Dm644 \
    "LICENSE-MIT" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE-MIT"
}

