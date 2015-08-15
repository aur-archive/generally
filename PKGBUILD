# vim:set ts=2 sw=2 et:

#Contributor: ezzetabi <ezzetabi at gawab dot com>

pkgname=generally
pkgver=1.2c
epoch=1
pkgrel=1
pkgdesc="GeneRally is a great, small racing game."
arch=(i686 x86_64)
url="http://gene-rally.com/"
license=('freeware')
makedepends=()
depends=(wine unionfs-fuse)
options=(!strip)
install=generally.install
source=(icons.tar.xz msvcp100.dll.tar.xz
 'generally.zip::http://gene-rally.com/download/latest/')
noextract=('icons.tar.xz' 'generally.zip')

build() {
  cd "$pkgdir"
  install -d -m 755 usr/share/generally usr/share/applications usr/bin \
    usr/share/pixmaps

  bsdtar xf "$srcdir"/icons.tar.xz
  ln -s /usr/share/icons/hicolor/48x48/apps/generally.png \
    usr/share/pixmaps/generally.png

  cd "$pkgdir"/usr/share/generally
  bsdtar xf  "$srcdir"/generally.zip
  install -m644 "$srcdir"/msvcp100.dll .

  cd "$pkgdir"
  find usr/ -type d -exec chmod 755 "{}" \;
  find usr/ -type f -exec chmod 644 "{}" \;

  #Lets create support files.
#############################################################################
  cd "$pkgdir"/usr/bin || return 1
  cat << EOF >generally
#!/bin/bash
set -e

function atexit {
  fusermount -zu "\$HOME"/.generally/generally/
  xrefresh
  echo Goodbye from GeneRally!
  exit 0
}

export WINEARCH=win32
export WINEPREFIX="\$HOME"/.generally/winefs
export WINEDLLOVERRIDES="mshtml="
export XDG_DATA_HOME=/dev/null
export WINEDEBUG=-all

echo Starting...

if [ ! -d "\$HOME"/.generally/ ] ;then
    mkdir -p "\$HOME"/.generally/generally
    cd "\$HOME"/.generally/
    mkdir generally_diff
    wineboot -u
    [ -x /usr/bin/winetricks ] && winetricks sandbox
    cd winefs/dosdevices
    ln -s ../../ x:
fi

cd /
unionfs -o cow "\$HOME"/.generally/generally_diff/=RW:/usr/share/generally=RO "\$HOME"/.generally/generally/
trap atexit SIGHUP SIGINT SIGTERM EXIT

wine 'x:\generally\GeneRally.exe' "\$@" &>/dev/null

EOF
  chmod 755 generally

#############################################################################
  cd "$pkgdir"/usr/share/applications
  cat <<EOF >generally.desktop
  [Desktop Entry]
  Version=1.0
  Exec=generally
  Icon=generally
  Type=Application
  Categories=Game;ActionGame;
  Name=Generally
  GenericName=Race game
  StartupNotify=true
  Terminal=false
EOF
  chmod 644 generally.desktop
#############################################################################

  return 0
}

md5sums=('8f5a1c01f8820ac45a0db57690233d00'
         'e4ecdf03883050fb65a199733ed7ef86'
         'b325c198d072c4fb5b32b7622f13fc7c')
