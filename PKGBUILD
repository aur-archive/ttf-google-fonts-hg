# Maintainer: Jason St. John <jstjohn .. purdue . edu>
# Contributor: Daniel Micay <danielmicay@gmail.com>
# Contributor: Michalis Georgiou <mechmg93@gmail.comr>
# Contributor: Alexander De Sousa <archaur.xandy21@spamgourmet.com>

#
#     NOTE:
#        ttf-google-fonts-hg (this package) pulls down the entire
#        Mercurial repository from the official upstream Web Fonts project.
#
#        ttf-google-fonts-git (an alternative package) pulls from a much
#        smaller and leaner, yet unofficial, repository hosted on GitHub.
#

pkgname=ttf-google-fonts-hg
pkgver=3776.0f6c13aa0c81
pkgrel=1
pkgdesc='TrueType fonts from the Google Fonts project (uses official upstream sources)'
arch=('any')
url='https://www.google.com/fonts'
license=('various')
# see comments in package() function about why "cantarell-fonts", "ttf-droid", and
# "ttf-inconsolata" are dependencies
depends=('cantarell-fonts'
         'fontconfig'
         'ttf-droid'
         'ttf-inconsolata'
         'xorg-fonts-encodings'
         'xorg-mkfontdir'
         'xorg-mkfontscale')
makedepends=('mercurial')
conflicts=('ttf-google-webfonts'
           'ttf-google-webfonts-git'
           'ttf-google-webfonts-hg'
           'adobe-source-code-pro-fonts'
           'adobe-source-sans-pro-fonts'
           'jsmath-fonts'
           'lohit-fonts'
           'otf-goudy'
           'ttf-andika'
           'ttf-anonymous-pro'
           'ttf-cantarell'
           'ttf-cardo'
           'ttf-chromeos-fonts'
           'ttf-comfortaa'
           'ttf-kimberly_geswein_print'
           'ttf-lato'
           'ttf-medievalsharp'
           'ttf-nova'
           'ttf-oldstandard'
           'oldstand-font'
           'ttf-opensans'
           'ttf-oxygen'
           'ttf-pt-mono'
           'ttf-ptsans'
           'ttf-pt-sans'
           'ttf-roboto'
           'ttf-sil-fonts'
           'ttf-sortsmillgoudy'
           'ttf-source-code-pro'
           'ttf-source-sans-pro'
           'ttf-ubuntu-font-family'
           'ttf-vollkorn')
provides=("${conflicts[@]}" 'ttf-font')
install=fonts.install
_hgname=googlefontdirectory
source=("${_hgname}::hg+https://googlefontdirectory.googlecode.com/hg/")
sha1sums=('SKIP')

pkgver() {
	cd "${_hgname}"
	hg identify -ni | awk 'BEGIN{OFS=".";} {print $2,$1}'
}

package() {
	cd "${_hgname}"
	install -dm755 "${pkgdir}/usr/share/fonts/TTF"
	find . -maxdepth 3 -type f -name \*.ttf -exec install -Dm644 '{}' \
		"${pkgdir}/usr/share/fonts/TTF" \;

	# install various font licenses
	#
	# each OFL license can be different due to how the OFL.txt files include
	# the copyright holder's name and Reserved Font Names at the beginning,
	# so we have to separately install each OFL.txt file
	find . -maxdepth 3 -type f -name OFL.txt -exec install -Dm644 '{}' \
		"${pkgdir}/usr/share/licenses/${pkgname}/{}" \;
	install -Dm644 'ufl/ubuntu/UFL.txt' \
		"${pkgdir}/usr/share/licenses/${pkgname}/UFL.txt"

	# remove Cantarell fonts because Google ships the original Cantarell
	# instead of the improved version of Cantarell shipped by the GNOME Project
	#
	# it is safe to remove "Cantarell-*.ttf" from this dir because the
	# cantarell-fonts package installs its fonts into /usr/share/fonts/cantarell/
	# and because cantarell-fonts installs .otf files instead of .ttf files
	find "${pkgdir}/usr/share/fonts/TTF" -type f -name "Cantarell-*.ttf" -delete

	# remove Inconsolata fonts because Google ships a modified version of
	# Inconsolata that some users do not prefer over the original Inconsolata
	# provided by the package "ttf-inconsolata"
	find "${pkgdir}/usr/share/fonts/TTF" -type f -name "Inconsolata-*.ttf" -delete

	# remove Droid fonts because the package "ttf-droid" provides several extra
	# variants of the Droid font
	find "${pkgdir}/usr/share/fonts/TTF" -type f -name "Droid*.ttf" -delete
}
