storage-graph
=============

Perl script that generates a visual representation of the overall
storage subsystem on a Linux system.

It would not be too hard to extend it to support other types of
unix platforms.

Subsystems Supported
--------------------
 * Linux physical devices (SATA and IDE-only at this stage)
 * DRBD (Linux Distributed Redundant Block Device; 'Network RAID1')
 * LVM2 (Linux Logical Volume Manager 2)
 * mdadm (Linux Software RAID)

Issues
------
 * Presently not well tested on other environments
 * Full of assumptions
 * Needs loving.
