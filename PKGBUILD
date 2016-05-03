# Contributor: Thomas Laroche <tho.laroche@gmail.com>
# Maintainer: Thomas Fanninger <thomas@fanninger.at>

_pkgname=gogs
_gourl=github.com/gogits/$_pkgname
pkgname=gogs
pkgver=0.9.13
pkgrel=1
pkgdesc="Gogs(Go Git Service) is a Self Hosted Git Service in the Go Programming Language. This is the latest release version."
arch=('i686' 'x86_64' 'armv6h' 'armv7h')
url="http://gogs.io/"
license=('MIT')
depends=('git>=1.7.1' 'bash')
optdepends=('sqlite: SQLite support'
            'mariadb: MariaDB support'
            'postgresql: PostgreSQL support'
            'redis: Redis support'
            'memcached: MemCached support'
            'openssh: GIT over SSH support'
            'tidb-git: TiDB support')
makedepends=('go>=1.4' 'git')
conflicts=('gogs')
provides=('gogs')
options=('!strip' '!emptydirs')
backup=('etc/gogs/app.ini')
install=gogs.install

source=('gogs.service'
        "$_pkgname::git+https://${_gourl}.git#tag=v${pkgver}")

prepare() {
    GOPATH="$srcdir/build"
    rm -rf "$GOPATH/src/github.com/gogits"
    mkdir -p "$GOPATH/src/github.com/gogits"
    mv  $_pkgname "$GOPATH/src/$_gourl"
}

build() {
    GOPATH="$srcdir/build"
    cd $GOPATH/src/${_gourl}
    go get -x -tags='sqlite pam cert'
    go fix
    go build -x -tags='sqlite pam cert'
}

package() {
    GOPATH="$srcdir/build"
    install -Dm0755 "$GOPATH/src/${_gourl}/$_pkgname" "$pkgdir/usr/share/$_pkgname/$_pkgname"
    cp -r "$GOPATH/src/${_gourl}/conf" "$pkgdir/usr/share/$_pkgname"
    install -dm755 "$pkgdir/usr/share/themes/gogs/default"
    cp -r "$GOPATH/src/${_gourl}/public" "$pkgdir/usr/share/themes/gogs/default"
    cp -r "$GOPATH/src/${_gourl}/templates" "$pkgdir/usr/share/themes/gogs/default"
    install -Dm0644 "$GOPATH/src/${_gourl}/conf/app.ini" "$pkgdir/etc/$_pkgname/app.ini"
    install -Dm0644 "$srcdir/gogs.service" "$pkgdir/usr/lib/systemd/system/gogs.service"
}

sha512sums=('2b468bf8208b53a61d7f3f0676b83de36467809ad703c168dcc275b0a0f0490308d9b451ca3fc35824db050f5323a394b239bafebf99c4bc8bcf61db12fab6e1'
            'SKIP')

