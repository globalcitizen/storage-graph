storage-graph
=============

Perl script that generates a visual representation of the overall
storage subsystem on a Linux system.

It would not be too hard to extend it to support other types of
unix platforms.

Supports:
 * DRBD (Linux Distributed Redundant Block Device; 'Network RAID1')
 * LVM2 (Linux Logical Volume Manager 2)
 * mdadm (Linux Software RAID)

Note:
 * Presently not well tested on other environments
 * Full of assumptions
 * Needs loving.