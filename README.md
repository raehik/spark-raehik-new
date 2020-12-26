## Ansible notes
  * All roles inherently depend on `base`.
  * I treat templates more like functions, I like to use local vars and pass in
    their values. Makes me feel more at ease.

## Notes
  * swap must be placed on a partition in order to be used for hibernation -
    there are workarounds, but it's maybe better to use a partition
    * https://www.kernel.org/doc/Documentation/power/swsusp-and-swap-files.txt
    * https://unix.stackexchange.com/questions/434177/why-a-file-as-swap-cant-be-used-for-hibernation-in-linux
    * https://askubuntu.com/questions/6769/hibernate-and-resume-from-a-swap-file/

## Post notes
### Secure Boot
The `boot` role prepares config for `sbupdate`, an AUR tool for generating UEFI
binaries and signing them (and managing it all via pacman hooks). Follow the
guide on https://github.com/andreyv/sbupdate to use. Overall:

  * generate your Secure Boot keys
  * probably load them, via `sbkeysync`? (TODO, try this next time)
    * looks like you do this by clearing certs first
  * place `.crt` and `.key` for db, (dbx if present), KEK, PK in `/etc/efi-keys`
  * `sudo sbupdate`

To add the generated UEFI binary to the EFI bootloader/config thing, use
`efibootmgr`:

    sudo efibootmgr -c -L "Arch Linux (Secure Boot)" -l /EFI/Linux/linux-signed.efi

And that should put it at the top too, so you should boot straight into it now.

Re-generating the UEFI binary when Linux is updated should be managed for you by
the installed pacman hooks.

### AUR packages
  * desktop: ttf-font-awesome-4
  * desktop: all Mozc stuff
  * desktop: tmuxinator
  * base: sbupdate

## Guides
For things that I can't or haven't yet automated with Ansible.

### RTL8821CE wireless radio
As of 2020-12-23, the RTL8821CE radio for my HP Pavilion 15-cw1104ng still isn't
in the Linux kernel. Luckily, it's easy to solve:

    yay -S rtl8821ce-dkms-git

This installs the necessary dependencies too. It'll take ages to build the first
time. But that's all you need. No rebuilding initramfs either.

### Japanese IME in Wayland
I use Sway (Wayland). Mozc is the only decent Linux Japanese IME out there. IMEs
in Wayland have never been any good. Here are my ad-hoc notes for getting Mozc
working in Sway.

  * I use IBus+Mozc. The other input methods seem kinda specific and/or shit.
  * Install via `yay -S ibus-mozc`. *(Note: See below for Emacs notes.)*
  * You need to add Mozc to IBus's input method list manually for some reason.
    With IBus running, run `/usr/lib/ibus-mozc/ibus-engine-mozc`, then run
    `ibus-setup` and add Mozc in the Input Method tab. Now you can terminate
    `ibus-engine-mozc`, and my IME cycle script should work.
  * To fix the weird breaking input bug I had from like Aug to Dec 2020, you
    gotta call `ibus-daemon` with `DISPLAY=`! So I suggest `DISPLAY= ibus-daemon
    --replace`. まだ入力とかは不器用なままだけどとりあえずやったぁー！
  * I recommend `emacs-mozc` (AUR) for Emacs users. `yay -S emacs-mozc`
    * Ugh: `emacs-mozc` is broken on AUR. I had to download the AUR snapshot,
      uncomment the "build emacs-mozc" line, and `makepkg` manually. I provide a
      patch in `etc/`, just download the snapshot from the AUR page: https://aur.archlinux.org/packages/emacs-mozc/ or direct may work: https://aur.archlinux.org/cgit/aur.git/snapshot/mozc.tar.gz

### Printing
Printing is generally awful, so I don't want to attempt to fake it via Ansible.
Instead, here are my solutions for printers I've used.

Install `cups` and `ghostscript` (appears pretty much required, but not an
enforced dependency) to start off. `systemctl enable cups.socket` will start
CUPS when a program attempts to use it -- handy. You'll still need `systemctl
start cups` when trying to access configure it.

  * **Epson WF-5110:** `epson-inkjet-printer-escpr` (AUR)
  * **Epson XP-312:**  `epson-inkjet-printer-escpr` (AUR)

Make sure to set the paper type if you're not American.

### i3status-rs
#### MPD
It's not as great as py3status or my own solutions. I had to install mpDris2:

    yay -S mpdris2

And I can't set volume, and the set width thing doesn't work with non-halfwidth
characters. But it does appear to work with Firefox/YouTube due to it exporting
a MPRIS interface or something, so that's a small win. It's good enough, so I'm
using it for now.

#### watson
Seems cools, trying it out. Easy install via user `pip`:

    pip install td-watson
