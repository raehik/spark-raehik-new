# Hostnames and naming conventions for installs
  * a *host* is a unique physical or virtual machine, the hardware
  * an *install* is a unique OS instance
    * this is what you should set the hostname to
  * a *host* can have/manage/boot multiple *installs*
  * installs may be bootable from multiple hosts - it depends on both sides
  * it's common to have a single install per host, so a good convention is to
    place the intended host name in the install name and use a generic install
    name like "arch"
  * you may wish to prefix info/further identifiers in the host name (not the
    install hostname!), such as whether it's physical or virtual, or important
    defining specs
