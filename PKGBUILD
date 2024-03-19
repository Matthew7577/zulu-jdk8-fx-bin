# Maintainer: Azure Zeng <weedycn@outlook.com>
# Contributor: Sam Guymer <sam at guymer dot me>
# Contributor: so1ar <so1ar114514@gmail.com>
# Contributor: Macnolo0x7D4 <yosoymacnolo@gmail.com>

_java_ver=8
_jdkname="zulu-jdk8-fx"
_zulu_build="${_java_ver}.76.0.17-ca"
pkgname="zulu-jdk8-fx-bin"
pkgver="${_java_ver}.0.402"
pkgrel=1
pkgdesc='Azul Zulu builds of OpenJDK are open source, TCK-tested and certified builds of OpenJDK.'
arch=('x86_64')
url='https://www.azul.com/downloads/'
license=('custom')
depends=(
  'java-runtime-common>=3'
  'ca-certificates-utils'
)
provides=(
  "java-environment=$_java_ver"
  "java-environment-openjdk=$_java_ver"
  "java-runtime-headless=$_java_ver"
  "java-runtime-headless-openjdk=$_java_ver"
  "java-runtime=$_java_ver"
  "java-runtime-openjdk=$_java_ver"
  "java-openjfx=$_java_ver"
)
options=(!strip)
install="$pkgname.install"
_tarballname="zulu${_zulu_build}-fx-jdk${pkgver}-linux_x64"
source=("https://cdn.azul.com/zulu/bin/${_tarballname}.tar.gz")
sha256sums=('dbd68302ef165584054a931416ae201bae01073fde8bf543320ef6c6ba41f70e')

_jvmdir="/usr/lib/jvm/${_jdkname}"

package() {
  cd "$srcdir/${_tarballname}"

  install -dm 755 "${pkgdir}/${_jvmdir}"
  cp -a . "${pkgdir}/${_jvmdir}/"

  # based on java-openjdk package_jdk-openjdk
  # https://github.com/archlinux/svntogit-packages/blob/3f6aa8ddd98f728a9b0701288a933d16f0e8bbaf/trunk/PKGBUILD

  # Set config files
  mv "${pkgdir}${_jvmdir}"/jre/lib/management/jmxremote.password{.template,}
  mv "${pkgdir}${_jvmdir}"/jre/lib/management/snmp.acl{.template,}

  # Conf
  install -dm 755 "${pkgdir}/etc/${_jdkname}"
  for f in "${_conf_files[@]}"; do
    _file="${_jvmdir}/jre/lib/$f"
    install -D -m 644 "${pkgdir}${_file}" "${pkgdir}/etc/${_jdkname}/$f"
    ln -sf "/etc/${_jdkname}/$f" "${pkgdir}${_file}"
  done

  # Install license
  install -d -m 755 "${pkgdir}/usr/share/licenses/${pkgbase}/"
  install -m 644 ASSEMBLY_EXCEPTION LICENSE THIRD_PARTY_README "${pkgdir}/usr/share/licenses/${pkgbase}"

  # Man pages
  for f in man/man1/* man/ja/man1/*; do
    install -Dm 644 "${f}" "${pkgdir}/usr/share/${f/\.1/-zulu-8.1}"
  done
  rm -rf "${pkgdir}/${_jvmdir}/man"
  ln -s /usr/share/man "${pkgdir}/${_jvmdir}/man"

  # Link JKS keystore from ca-certificates-utils
  rm -f "${pkgdir}${_jvmdir}/jre/lib/security/cacerts"
  ln -sf /etc/ssl/certs/java/cacerts "${pkgdir}${_jvmdir}/jre/lib/security/cacerts"

}
