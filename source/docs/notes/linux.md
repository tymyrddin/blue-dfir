# Linux forensics

## Analysis of storage layout and volume management

### Analysis of partition tables
Identify the partition scheme, analyse the partition tables, and look for possible inter-partition gaps.

A DOS partition table entry allocates one byte for the partition type. No authoritative standards body defines DOS partition types, but there is a [community effort to maintain a list of known partition types](https://www.win.tue.nl/~aeb/partitions/partition_types-1.html).

A GPT partition table entry allocates 16 bytes for the partition GUID. [The Linux Discoverable Partitions Specification](https://systemd.io/DISCOVERABLE_PARTITIONS/) defines several Linux GUID partition types, but it is not complete. **Note**: Don’t confuse the standard defined GUID of the partition type with the randomly generated GUID that is unique to a particular partition or filesystem.

On a Linux system, detected partitions appear in the `/dev/` directory. This is a mounted pseudo-directory on a running system. In a postmortem forensic examination, this directory will be empty, but the device names may still be found in logs, referenced in configuration files, or found elsewhere in files on the filesystem.

### Logical volume manager

The most common volume manager in Linux environments is LVM. Extents are similar to traditional filesystem blocks, and have a fixed size defined at creation. A typical default LVM extent size is 8192 sectors (4MB) and is used for both Physical extents (PEs) and Logical extents (LEs). LVM also provides redundancy and stripping for logical volumes.

The use of partition tables is not required for LVM, and PVs can be created directly on the raw disk without a partition. When partitions are used, LVM has a partition entry type indicating that the physical drive is a PV.

More complex scenarios involving multiple drives will require forensic tools that support LVM volumes or a Linux forensic analysis machine able to access and assemble LVM volumes.

### RAID

Some commercial forensic tools may support the reassembly and analysis of Linux RAID systems.

RAID capability in Linux can be provided by md (multiple device driver, or Linux Software RAID), the LVM, or built in to the filesystem (`btrfs` and `zfs` have integrated RAID capability, for example). 

The most commonly used method of RAID is the Linux software RAID or `md`. This kernel module produces a meta device from a configured array of disks. The `mdadm` userspace tool is then used to configure and manage the RAID.

A disk used in a RAID may have a partition table with standard Linux RAID partition types. For DOS/MBR partition tables, the partition type for Linux RAID is `0xFD`. A forensic tool will find these partitions on each disk that is part of a RAID system.

Each device from a Linux RAID system has a superblock (not to be confused with filesystem superblocks, which are different) that contains information about the device and the array. The default location of the `md` superblock on a modern Linux RAID device is eight sectors from the start of the partition. It can be recognised by the magic string `0xA92B4EFC`. This superblock information can be examined with a hex editor or the `mdadm` command.

The disks in an array might not all be identical sizes. For a forensic examination, this can be important.

The array device is typically in the form `/dev/md#`, `/dev/md/#`, or `/dev/md/NAME`. These kernel devices will exist only on a running system, but in a postmortem forensic examination, they may be found in the logs. They can also be defined in separate configuration files. During an examination involving RAID systems, check for un- commented `DEVICE` or `ARRAY` lines in the `/etc/mdadm.conf` file (or files in `/etc/mdadm.conf.d/`).

## Filesystem forensic analysis