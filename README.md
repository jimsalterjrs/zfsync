zfsync is a Perl wrapper for zfs send | zfs receive.

It's intended to make synchronizing a local ZFS filesystem with a remote one 
much easier and less painful.

Currently, zfsync requires a (preferably passwordless) root SSH login enabled
on the remote box, and must be run as root on the local box.  Synchronization
MUST be "pull", ie remote source, local target - "push" synch is not currently
supported.

zfsync requires Config::IniFiles for Perl (available in Debian based systems
with apt-get install libconfig-inifiles-perl), and will make much much
prettier progress indication for you if pv (available on Debian based systems
with apt-get install pv) is available.

usage: zfsync remotehost:remotezpool/remotezfs localzpool/localzfs

You may also append a "-z" as a third argument if you wish to use gzip stream
compression (usually highly recommended).




TODO: write MOAR WRAPPER! so that zfsync can operate as a neutered user
      which only has sudo privileges to run zfsync itself, so that remote
      source systems can expose zfsync without exposing themselves to either
      remote root access OR any zfs operations OTHER than the ones
      necessary to run zfsync itself.

      parse arguments in a non-brain-dead fashion.

      package as a .deb for easy installation.

      write a man page.
