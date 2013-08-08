zfsync is a Perl wrapper for zfs send | zfs receive, with built-in pruning of snapshots
to maintain a minimum free space level on the target.  It's intended to make synchronizing 
a local ZFS filesystem with a remote one much simpler and less painful.

Usage: zfsync [user@]remotehost:sourcepool/sourcefs targetpool/targetfs [minpercentfree]

===========================================================================================

zfsync will create snapshots on the source in the form @zfsync_remotehostname_timestamp.  
This allows you to use zfsync to sync the same source to multiple targets without the 
targets stomping on each others' snapshots.

zfsync will use ANY matching snapshots between target and source, not just zfsync snapshots.  
This allows, for example, incremental progress from overall failed zfsync runs - if you have 
hourly snapshots on the source and run zfsync once per day, a daily zfsync run that crashes or 
is killed halfway through will have probably successfully replicated 12 of the 24 hourly 
snapshots, and the next  zfsync run can pick up from the latest one instead of starting all 
over from the last zfsync_remotehost snapshot.

[minpercentfree] is the minimum percentage of the target which must be free before replication 
will occur.  zfsync will prune non-zfsync snapshots from oldest to newest in order to try to 
create this space, and die if there still isn't enough after all but one (non-zfsync) snapshot 
has been pruned.  If minpercentfree is not specified, 0.1 (10%) is assumed.

Currently, zfsync requires a (preferably passwordless) root SSH login enabled on the remote 
box, and must be run as root on the local box.  Synchronization MUST be "pull", ie remote 
source, local target - "push" sync is not currently supported.

zfsync will automatically find and use Andrew Woods' excellent utility pv if available,
and will also automatically find and use mbuffer on the local (target) host if available.
mbuffer can be used on the remote host if you flip a bit in your local copy of zfsync,
but it's disabled by default until I write a routine to automatically look for it on 
the remote host.


      TODO: 
      =====
      
      write MOAR WRAPPER! so that zfsync can operate as a neutered user
      which only has sudo privileges to run zfsync itself, so that remote
      source systems can expose zfsync without exposing themselves to either
      remote root access OR any zfs operations OTHER than the ones
      necessary to run zfsync itself.

      write routines to look for zfs and gzip on the remote host instead of 
      assuming they will be in the same place as on the local host, and to 
      look for and automatically use a remote copy of mbuffer as well as a 
      local one if available.

      parse arguments in a non-brain-dead fashion.

      package as a .deb for easy installation.

      write a man page.
