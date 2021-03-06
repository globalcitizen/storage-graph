storage-graph
=============

Perl script that generates a visual representation of the overall
storage subsystem on a Linux system.

It would not be too hard to extend it to support other types of
unix platforms.

Design Notes
------------
This was written as a practical hack. I was rather surprised to
find no cross-platform way of querying for such information
already available under perl; or indeed any other standard UNIX
scripting language, as far as I could tell at the time.  There
may well be something out there; I just didn't find it, and
needed some more exotic support (DRBD, nested LVM2, etc.)

Subsystems Supported
--------------------
 * Linux physical devices (SATA and IDE-only at this stage)
 * DRBD (Linux Distributed Redundant Block Device; 'Network RAID1')
 * LVM2 (Linux Logical Volume Manager 2)
 * mdadm (Linux Software RAID)

Issues
------
 * Only works on PC partition tables (not GPT)
 * Errors if no RAID present
 * Presently not well tested on other environments
 * Full of assumptions
 * Needs loving.

Dependencies
------------
 * Perl
    * Graph::Easy
    * POSIX
    * Number::Bytes::Human
 * System-level
    * hdparm
    * LVM2 userland utilities
    * df
    * Normal (POSIXy) grep, sed, etc.

Sample output
-------------
This output shows a system where there are two primary physical SATA volumes, */dev/sda* and */dev/sdb*.  Each of these have two partitions.  One of these partitions, */dev/sdb2*, is used as the basis for an LVM2 volume group, */dev/VolumeGroup*.  On top of this volume group rests an LVM2 volume called */dev/VolumeGroup/DRBDContainer*, which contains the DRBD volume */dev/drbd/drbd1* (resource id *r0*).

<pre>
+ - - - - - - - - - - - - - - - - - - -+
' physical disks                       '
'                                      '
' +--------------+       +-----------+ '
' |     disk     |       | partition | '
' |     /dev/sda |       | /dev/sda1 | '
' |        (1TB) | ----> |   (512MB) | '
' +--------------+       +-----------+ '
'   |                                  '
'   |               - - - - - - - - - -+
'   |              '
'   |              '
'   v              '
' +--------------+ '
' |  partition   | '
' |    /dev/sda2 | '
' |        (1TB) | '
' +--------------+ '
'                  '
'                  '                                                     +--------------------------------------+
'                  '                                                     |                                      |
'                   - - - - - - - - - -+                                 |
'                                      '                                 |
'                                      '                                 v
' +--------------+       +-----------+ '     +-------------------+     +--------------------------------+     +---------------+
' |     disk     |       | partition | '     | lvm2 volume group |     |      lvm2 logical volume       |     | drbd resource |
' |     /dev/sdb |       | /dev/sdb2 | '     |  /dev/VolumeGroup |     | /dev/VolumeGroup/DRBDContainer |     |    /dev/drbd1 |
' |        (1TB) | ----> |     (1TB) | ' --> |                   | --> |                                | --> |          (r0) |
' +--------------+       +-----------+ '     +-------------------+     +--------------------------------+     +---------------+
'   |                                  '
'   |               - - - - - - - - - -+
'   |              '
'   |              '
'   v              '
' +--------------+ '
' |  partition   | '
' |    /dev/sdb1 | '
' |      (502MB) | '
' +--------------+ '
'                  '
+ - - - - - - - - -+
</pre>
