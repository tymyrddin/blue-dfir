# Linux acquisition platform

The primary advantage of using OSS in a forensic context is transparency. Unlike proprietary commercial software, the source code can be reviewed and openly validated. In addition, researchers can study it and build on the work of others in the community.

## Filesystem

[The Linux kernel supports a large number of filesystems](https://en.wikipedia.org/wiki/Category:File_systems_supported_by_the_Linux_kernel), which can be useful when performing some DFIR tasks. Filesystem support is not necessary when performing forensic acquisition, because the imaging process is operating on the block device below the filesystem and partition scheme.

To provide a consistent interface for different types of filesystems, the Linux kernel implements a Virtual File System (VFS) abstraction layer. This allows mounting of regular storage media filesystems (`EXT*`, `NTFS`, `FAT`, ...), network-based filesystems (`nfs`, `sambafs/smbfs`, ...), userspace filesystems based on [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace), stackable filesystems (`encryptfs`, `unionfs`, ...), and other special pseudo filesystems (`sysfs`, `proc`, ...).

### Mounting

Filesystems that reside on disk devices in Unix and Linux require explicit mounting before being accessible as a regular directory structure. Mounting a filesystem means it is made available to use with standard file access tools (file managers, applications, ...), similar to drive letters in the DOS/Windows world. Linux doesn’t use drive letters; mounted disks become part of the local filesystem and are attached to any chosen part of the filesystem tree, the mount point.

To mount a USB stick on an investigator system using (`/mnt`) as the mount point:

```shell
sudo mount /dev/sdb1 /mnt
```

To unmount, use the `umount` command (not unmount) with either the device name or the mount point.

```shell
sudo umount /dev/sdb1
sudo umount /mnt
```

After the filesystem is unmounted, the raw disk is still visible to the kernel and accessible by block device tools, even though the filesystem is not mounted. An unmounted disk is safe to physically detach from an investigator’s acquisition system.

***Do not attach or mount suspect drives without a write blocker***. There is a high risk of modifying, damaging, and destroying digital evidence. Modern OSes will update the last-accessed timestamps as the files and directories
are accessed. Any userspace daemons (search indexers, thumbnail generators, and so on) might write to the disk and overwrite evidence, filesystems might attempt repairs, journaling filesystems might write out journal data, and other human accidents might occur.

In cases where the Linux kernel does not detect the filesystem, it may have to be explicitly specified. A filesystem will not be correctly detected for any of the following reasons:

* The filesystem is not supported by the host system (missing kernel module or unsupported filesystem).
* The partition table is corrupted or missing.
* The partition has been deleted.
* The filesystem offset on the disk is unknown.
* The filesystem needs to be made accessible (unlock device, decrypt partition, and so on).

## Forensic evidence container

The technique described here uses SquashFS as a forensic evidence container together with a small shell script, `sfsimage`, which manages aspects of the container. This method creates a compressed image combined with imaging logs, information about the disk device, and any other information(photographs, chain of custody forms, and so on) into a single package. The files are contained in a read-only SquashFS filesystem, which can be accessed without any special forensic tools.

### SquashFS

SquashFS is a highly compressed, read-only filesystem written for Linux. It was created by Phillip Lougher in 2002 and was merged into the Linux kernel tree in 2009, starting with kernel version 2.6.29.

* It is read-only; items can be added but not removed or modified.
* It stores investigator’s uid/gid and creation timestamps.
* It supports very large file sizes (theoretically up to 16EiB).
* It is included in the Linux kernel and trivial to mount as a read-only filesystem.
* The filesystem is an open standard (tools exist for Windows, OS X).
* The `mksquashfs` tool uses all available CPUs to create a container.

Using SquashFS as a forensic evidence container is a practical alternative to using other forensic formats, because it facilitates the management of compressed raw images acquired with `dd`. 

### sfsimage

The [sfsimage](https://digitalforensics.ch/sfsimage/) tool provides the functionality to manage SquashFS forensic evidence containers. To configure it, you can edit the script or create separate `sfsimage.conf` files. The config file is documented with comments and examples, and it allows defining:

* Preferred imaging/acquisition command (`dd`, `dcfldd`, `dc3dd`, ...)
* Preferred command to query a device (`hdparm`, `tableu-parm`, ...)
* Default directory to mount the evidence container (the current working directory is the default)
* How to manage privileged commands (`sudo`, `su`, ...)
* Permissions and uid/gid of created files

To image a disk into a SquashFS forensic evidence container, run `sfsimage` using the `-i` flag, the disk device, and the name of the evidence container:

```shell
sfsimage -i /dev/sde containername.sfs
```

To add additional evidence to a container, use the `-a` flag:

```shell
sfsimage -a photo.jpg containername.sfs
```

List the contents of the container with the `-l` flag:

```shell
sfsimage -l containername.sfs
```

This command output shows the contents of the `containername.sfs` container (without mounting it). Also shown are the correct times when the files were created or added. The error log, hash log, and sfsimage log contain documentation about activity and errors.

Mount the container with the `-m` flag:

```shell
sfsimage -m containername.sfs
```

```shell
containername.sfs.d mount created
```

When the mount is no longer needed, unmount it with the `-u` flag:

```shell
sfsimage -u containername.sfs.d
```

## Loop devices

The basis for many methods is the Linux loop device, a pseudo device that can be associated with a regular file, making the file accessible as a block device in `/dev`.

While being a virtual file system, there are endless possibilities; here are some widely known use cases of loop devices:

* It can be used to install an operating system over a file system without going through repartitioning the drive.
* A convenient way to configure system images (after mounting them).
* Provides permanent segregation of data.
* It can be used for sandboxed applications that contain all the necessary dependencies.

If you are an Ubuntu user, then you’ll have a long list of loop devices, because of snaps, the universal package management system developed by Canonical. The snap applications are mounted as loop devices.

If not that, Linux systems typically create eight loop devices by default, which might not be enough for a forensic acquisition host, but this number can be increased on boot up. To create 32 loop devices during boot up, add `max_loop=32` to the `GRUB_CMDLINE_LINUX_DEFAULT=` line in the `/etc/default/grub` file:

```shell
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash max_loop=32"
```

After reboot, 32 unused loop devices should be available. The sfsimage script uses loop devices to mount SquashFS forensic evidence containers.
