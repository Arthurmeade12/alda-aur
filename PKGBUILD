# Maintainer: Arthurmeade12 <s728c3ilp@relay.firefox.com>
# Former Maintainer: Luis Martinez <luis dot martinez at disroot dot org>
# Former Contributor: ml <>

pkgname=alda
pkgver=2.2.5
pkgrel=1
pkgdesc='A music programming language for musicians'
arch=('x86_64')
url='https://github.com/alda-lang/alda'
license=('EPL')
depends=('bash' 'java-runtime>=8')
makedepends=('go' 'gradle')
<<<<<<< HEAD
source=("$pkgname-release-$pkgver.tar.gz::https://codeload.github.com/$pkgname-lang/$pkgname/tar.gz/refs/tags/release-$pkgver" 
	"$pkgname-player::https://$pkgname-releases.nyc3.digitaloceanspaces.com/$pkgver/player/non-windows/$pkgname-player")
||||||| f9142c7 (Add URL for alda-player)
source=("$pkgname-release-$pkgver.tar.gz::https://codeload.github.com/$pkgname-lang/$pkgname/tar.gz/refs/tags/release-$pkgver" 
	"https://$pkgname-releases.nyc3.digitaloceanspaces.com/$pkgver/player/non-windows/$pkgname-player")
=======
source=("$pkgname-release-$pkgver.tar.gz::https://codeload.github.com/$pkgname-lang/$pkgname/tar.gz/refs/tags/release-$pkgver"
        alda-player)
>>>>>>> parent of f9142c7 (Add URL for alda-player)
sha256sums=('5895896dcaea7678ae6aeefae5c49c548ff7bd23d2337985e8c1bb7fe431898d'
            'bde2d7c169cdc0d59b6ed3f8babbfa056f3a9e640adc34ea868fb9510e881c9c')

prepare() {
	cd "$pkgname-release-$pkgver/client"
	go mod download
}

build() {
	cd "$pkgname"-release-"$pkgver"
	(
		cd client
		export CGO_ENABLED=1
		export CGO_LDFLAGS="$LDFLAGS"
		export CGO_CFLAGS="$CFLAGS"
		export CGO_CPPFLAGS="$CPPFLAGS"
		export CGO_CXXFLAGS="$CXXFLAGS"
		export GOFLAGS='-buildmode=pie -modcacherw -trimpath -ldflags=-linkmode=external'
		go generate
		go build -o "$pkgname"
	)

	cd player
	gradle --no-daemon --parallel build
}

package() {
	install -Dv alda-player -t "$pkgdir/usr/bin"

	cd "$pkgname"-release-"$pkgver"
	install -Dv client/alda -t "$pkgdir/usr/bin"
	install -Dvm644 player/build/libs/alda-player-fat.jar -T "$pkgdir/usr/share/java/alda-player.jar"
	install -Dvm644 doc/* -t "$pkgdir/usr/share/doc/$pkgname/"

	# EPL v2 is not part of core/licenses. Let's add it here
	install -Dm644 LICENSE -t "$pkgdir/usr/share/licenses/$pkgname/"
}
