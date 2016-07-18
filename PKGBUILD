# Contributor: Thomas Laroche <tho.laroche@gmail.com>
# Maintainer: Thomas Fanninger <thomas@fanninger.at>

_pkgname=gogs
_gourl=github.com/gogits
pkgname=gogs
pkgver=0.9.48
pkgrel=1
pkgdesc="Self Hosted Git Service in the Go Programming Language. This is the latest release version."
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url="http://gogs.io/"
license=('MIT')
depends=('git>=1.7.1' 'bash')
optdepends=('sqlite: SQLite support'
            'mariadb: MariaDB support'
            'postgresql: PostgreSQL support'
            'redis: Redis support'
            'memcached: MemCached support'
            'openssh: GIT over SSH support')
makedepends=('go>=1.4' 'git' 'glide')
conflicts=('gogs')
provides=('gogs')
options=('!strip' '!emptydirs')
backup=('etc/gogs/app.ini')
install=gogs.install

source=('gogs.service'
        "$_pkgname::git+https://${_gourl}/${_pkgname}.git#tag=v${pkgver}")

prepare() {
    GOPATH="$srcdir/build"
    rm -rf "$GOPATH/src/${_gourl}"
    mkdir -p "$GOPATH/src/${_gourl}"
    mv  ${_pkgname} "$GOPATH/src/${_gourl}/${_pkgname}"
    cd "$GOPATH/src/${_gourl}/${_pkgname}"
    glide install
}

build() {
    GOPATH="$srcdir/build"
    cd "$GOPATH/src/${_gourl}/${_pkgname}"
    go fix
    go build -x -tags='sqlite pam cert'
}

package() {
    GOPATH="$srcdir/build"
    install -Dm0755 "$GOPATH/src/${_gourl}/${_pkgname}/${_pkgname}" "$pkgdir/usr/share/${_pkgname}/${_pkgname}"
    cp -r "$GOPATH/src/${_gourl}/${_pkgname}/conf" "$pkgdir/usr/share/${_pkgname}"
    install -dm755 "$pkgdir/usr/share/themes/gogs/default"
    cp -r "$GOPATH/src/${_gourl}/${_pkgname}/public" "$pkgdir/usr/share/themes/gogs/default"
    cp -r "$GOPATH/src/${_gourl}/${_pkgname}/templates" "$pkgdir/usr/share/themes/gogs/default"
    install -Dm0644 "$GOPATH/src/${_gourl}/${_pkgname}/conf/app.ini" "$pkgdir/etc/${_pkgname}/app.ini"
    install -Dm0644 "$srcdir/gogs.service" "$pkgdir/usr/lib/systemd/system/gogs.service"
}

sha512sums=('95c697a38ebf6a6a2c65fd10aa0ecf1a4aa62f41b5224012366cd0976e20876b02ed1422b00adb4a9fe6f199be2fb6ab0ca7cbd9565e3a790ccfd4627b39d586'
            'SKIP')
