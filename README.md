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
