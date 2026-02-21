# Maintainer: Stephano Cetola <stephano@cetola.net>
pkgname=reform-tools
pkgver=1.79
pkgrel=1
pkgdesc="MNT Reform system tools & helpers"
arch=('x86_64' 'aarch64')
url="https://source.mnt.re/reform/reform-tools"
license=('GPL3')
install=reform-tools.install


depends=(
  'python'
  'python-psutil'
  'i2c-tools'
  'help2man'
)
optdepends=(
  'mtd-utils: for NAND flashing tools'
  'alsa-utils: for audio-related tools'
  'lm_sensors: for sensor monitoring'
  'ircii: for Reform chat/IRC tools'
  'pavucontrol: GUI mixer control (if using PulseAudio)'
)

source=(
  "git+https://source.mnt.re/reform/reform-tools.git#tag=$pkgver"
  'motd-full'
  'motd-rescue'
)
sha256sums=(
  'SKIP'
  '5df16d0ad47909ffc6a7a890d9814ec0749f13707f5343466264d4fbe2d46f1c'
  'f7986b5dce945a9dcc923bbc66db11f6f5cba1e78576873b644fadff4dc5ad8d'
)

build() {
  cd "$srcdir/reform-tools"
  make
}

package() {
  cd "$srcdir/reform-tools"

  # Binaries (from bin/)
  install -d "$pkgdir/usr/bin"
  install -m755 bin/* "$pkgdir/usr/bin/"


  install -Dm644 systemd/reform-hw-setup.service "$pkgdir/usr/lib/systemd/system/reform-hw-setup.service"

  # MOTD files
  install -Dm644 "$srcdir/motd-full"   "$pkgdir/etc/motd-full"
  install -Dm644 "$srcdir/motd-rescue" "$pkgdir/etc/motd-rescue"

  # modprobe.d blacklists
  install -Dm644 modprobe.d/reform.conf "$pkgdir/usr/lib/modprobe.d/reform.conf"

  # NetworkManager Wi-Fi power-save off
  install -Dm644 NetworkManager/default-wifi-powersave-off.conf \
    "$pkgdir/usr/lib/NetworkManager/conf.d/default-wifi-powersave-off.conf"

  # UCM2 configs (RK3588 + TLV320AIC3100)
  install -Dm644 audio/ucm2.conf.d/rk3588-tlv320ai/rk3588-tlv320aic3100.conf \
    "$pkgdir/usr/share/alsa/ucm2/rk3588-tlv320ai/rk3588-tlv320aic3100.conf"
  install -Dm644 audio/ucm2.conf.d/rk3588-tlv320ai/HiFi.conf \
    "$pkgdir/usr/share/alsa/ucm2/rk3588-tlv320ai/HiFi.conf"

  # WirePlumber HDMI audio priority
  install -Dm644 audio/reform-hdmi-audio-priority.conf \
    "$pkgdir/usr/share/wireplumber/wireplumber.conf.d/reform-hdmi-audio-priority.conf"

  # GNOME defaults (gschema override)
  install -Dm644 schemas/20_reform.gschema.override \
    "$pkgdir/usr/share/glib-2.0/schemas/20_reform.gschema.override"

  # Dracut config for Reform
  install -Dm644 dracut/20-pocket-reform.conf \
    "$pkgdir/usr/lib/dracut/dracut.conf.d/20-pocket-reform.conf"

  # Machine configs for reform-* tools
  install -d "$pkgdir/usr/share/reform-tools/machines"
  install -Dm644 machines/* "$pkgdir/usr/share/reform-tools/machines/"

  # Backgrounds for GNOME defaults/reform-gnome-config
  install -d "$pkgdir/usr/share/backgrounds"
  install -Dm644 share/backgrounds/* "$pkgdir/usr/share/backgrounds/"
}
