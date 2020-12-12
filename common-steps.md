## Prepare installation medium
  1. https://www.archlinux.org/download/ via torrent (magnet link)
  2. `sudo dd bs=4M if=$ARCH_ISO_PATH of=$INSTALL_DEVICE status=progress oflag=sync`
    * TODO: I'd use cat but `sudo cat >` doesn't work...
  3.
