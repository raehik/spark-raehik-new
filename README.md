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

## Notes
  * swap must be placed on a partition in order to be used for hibernation -
    there are workarounds, but it's maybe better to use a partition
    * https://www.kernel.org/doc/Documentation/power/swsusp-and-swap-files.txt
    * https://unix.stackexchange.com/questions/434177/why-a-file-as-swap-cant-be-used-for-hibernation-in-linux
    * https://askubuntu.com/questions/6769/hibernate-and-resume-from-a-swap-file/

## Ansible notes
  * All roles inherently depend on `base`.
  * I treat templates more like functions, I like to use local vars and pass in
    their values. Makes me feel more at ease.
