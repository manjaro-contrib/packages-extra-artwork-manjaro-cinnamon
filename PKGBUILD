# Contributor: Bernhard Landauer <oberon@manjaro.org>

pkgname=('artwork-cinnamon-minimal' 'cinnamon-wallpapers')
pkgbase=artwork-manjaro-cinnamon
pkgver=20190729
pkgrel=2
_grepo=gnome-backgrounds
_gver=3.32.0
pkgdesc="Wallpapers for Manjaro Cinnamon"
arch=('any')
url="https://github.com/oberon-manjaro/artwork-manjaro-cinnamon"
license=('GPL-3.0-or-later')
makedepends=('git' 'glib2' 'meson')
_commit=ced74fb432e04f9fea20ddb4e7d8a6d2e2cab12a
source=("https://download.gnome.org/sources/gnome-backgrounds/${_gver:0:4}/${_grepo}-${_gver}.tar.xz"
        "git+https://github.com/oberon-manjaro/artwork-manjaro-cinnamon.git#commit=${_commit}")
sha256sums=('3f53c31bc9ff31b27408159afe77d06875174bc554e4e7c225a4d5d16acbb3e8'
            '5f2e9babd204b75da01f91930af5def3a91b0c925cf8ba997dbd79efafe27e16')

_greeter_default="cinnamon-adapta-maia.jpg"  # default greeter background
_default=${_greeter_default}  # default wallpaper to be packaged in artwork-minimal
_target="/usr/share/backgrounds/cinnamon"

pkgver() {
  cd "$pkgbase"
  printf "$(git show -s --format=%cd --date=format:%Y%m%d HEAD)"
}

prepare() {
  cd "${_grepo}-${_gver}"

  # Function does not take positional arguments
  sed -i 's/i18n.merge_file(metadata,/i18n.merge_file(/g' backgrounds/meson.build
}

build() {
  arch-meson "${_grepo}-${_gver}" build
  meson compile -C build
}

package_artwork-cinnamon-minimal() {
  depends=('manjaro-icons')
  optdepends=('cinnamon-wallpapers: more wallpapers for Manjaro Cinnamon')
  provides=("${_grepo}")
  conflicts=("${_grepo}")

  _ext=$(echo ${_default} | cut -d'.' -f2)
  _ext2=$(echo ${_greeter_default} | cut -d'.' -f2)

  meson install -C build --destdir "$pkgdir"

  # fix default dir listing
  cd "$pkgdir"/usr/share/gnome-background-properties
  rm adwaita.xml
  mv gnome-backgrounds.xml gnome.xml

  cd "$srcdir/$pkgbase"
  install -Dm644 "backgrounds/${_default}" "$pkgdir/${_target}/${_default}"
  install -m644 "backgrounds/${_greeter_default}" "$pkgdir/${_target}/${_greeter_default}"
  ln -s "${_target}/${_default}" "$pkgdir/usr/share/backgrounds/cinnamon_default.${_ext}"
  ln -s "${_target}/${_greeter_default}" "$pkgdir/usr/share/backgrounds/greeter_default.${_ext2}"

  cp -r icons "$pkgdir"/usr/share/
  ln -s /usr/share/icons/manjaro/maia/48x48.png "$pkgdir"/usr/share/icons/manjaro-maia.png
}

package_cinnamon-wallpapers() {
  cd "$pkgbase"
  make -C backgrounds install DESTDIR="$pkgdir"

  rm "$pkgdir/${_target}/${_greeter_default}"
  # rm "$pkgdir/${_target}/${_default}"
}
