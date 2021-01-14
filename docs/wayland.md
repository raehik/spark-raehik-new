# Wayland notes, troubleshooting, using
## Japanese IME
Mozc is the only decent Linux Japanese IME out there. IMEs in Wayland have never
been any good. Here are my ad-hoc notes for getting Mozc working in Sway.

TODO: actually, try `skk` too!

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

## Screen sharing
As of 2021-01-14, it actually works! In Firefox! With no configuration! All you
need is "one" package (will install `pipewire` dependency):

    pacman -S xdg-desktop-portal-wlr

Now -- maybe after a restart -- when you try to screen share it should let you
pick your entire screen. Some things to keep in mind:

  * You can't pick individual windows.
  * You can't pick the screen -- it's set by the portal
  * (I didn't have to set any `XDG_CURRENT_DESKTOP`.)

The screen is the big one. On my laptop, it shows my laptop screen if no
external monitor is plugged, or the external monitor if it is. Very dynamic, no
pain there. If you want to pick it on-the-fly, I suggest this:

  * Kill the existing `xdg-desktop-portal-wlr` process.
  * Run (in foreground!): `/usr/lib/xdg-desktop-portal-wlr --output=$OUTPUT`

Then quit that process when you're done, and it'll go back to its default config
and continue to be socketed/start up correctly, normally. Example shell command:

    pkill --echo --full xdg-desktop-portal-wlr ; /usr/lib/xdg-desktop-portal-wlr --output=eDP-1

I have zero issues with hotswapping and whatnot, `xdg-desktop-portal-wlr` always
gets run if it's not already. Great!
