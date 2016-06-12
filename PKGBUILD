pkgname=shinken-modwebui2
_gitname=mod-webui
_gitowner=shinken-monitoring
_moddir=webui2
pkgver=2.3.2
pkgrel=1
pkgdesc='Webui module for shinken, latest release'
arch=('any')
license=('custom')
depends=('shinken')
makedepends=('curl' 'bc')
backup=('etc/shinken/modules/webui2.cfg')
source=("git+https://github.com/$_gitowner/$_gitname/")
md5sums=('SKIP')

pkgver(){
  curl -s "https://api.github.com/repos/$_gitowner/$_gitname/releases/latest" | grep tag_name | sed -e 's/  "tag_name": "//' -e 's/".*//'
}

package() {
  cd $_gitname
  git checkout tags/$pkgver -b $pkgver
  mkdir -p $pkgdir/var/lib/shinken/{modules,inventory,doc/source/89_packages}/
  mkdir -p $pkgdir/var/lib/shinken/inventory/$_moddir
  chmod -R o-rwx .
  echo Creating $pkgdir/var/lib/shinken/inventory/$_moddir/content.json
  find . | while read fic
  do
    mode8=$(stat $fic --printf=%a)
    mode10=$(echo "obase=8; ibase=10; $mode8" | bc)
    LANG=C stat $fic --printf='{"size":"%s", "type":"%t", "name":"%n", "mode","'$mode8'"},'
  done > $pkgdir/var/lib/shinken/inventory/$_moddir/content.json
  sed -i 's/^\(.*\),$/[\1]/' $pkgdir/var/lib/shinken/inventory/$_moddir/content.json
  mv module $pkgdir/var/lib/shinken/modules/$_moddir
  mv doc $pkgdir/var/lib/shinken/doc/source/89_packages/$_moddir
  mv package.json $pkgdir/var/lib/shinken/inventory/$_moddir
  mkdir -p $pkgdir/etc/shinken/modules/
  mv etc/modules/$_moddir.cfg $pkgdir/etc/shinken/modules/
  chmod 0640 $pkgdir/etc/shinken/modules//$_moddir.cfg
}

# vim:set ts=2 sw=2 et:
