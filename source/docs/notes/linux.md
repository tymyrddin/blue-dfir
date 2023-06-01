# Linux analysis

## Shells

The term Linux technically refers only to the kernel, which is the actual OS. Graphical interface, tools and utilities, and even the command line shell are not Linux but parts of a Linux distribution. A distribution is a functional package that typically contains the Linux kernel, installers and package managers (usually unique to the distribution), and programs and utilities (including standard applications, such as Office suites, web browsers, email/chat clients, ...). 

The shell is a command prompt that humans and/or machines use to submit commands to instruct and control an OS. The shell starts or stops programs, installs software, shuts down a system, and performs other work. The command shell offers more powerful features and possibilities than graphical environments.

The most common shell in use today, and the default in most Linux distributions, is Bash.

### Shell history

The Unix/Linux shell was not originally designed with logging or audit trails in mind. In the past, patches have been created to augment the history mechanism and hacks have attempted to capture commands as the shell is used. Using basic Bash shell history as an audit trail is rudimentary. The Bash shell history can be configured to satisfy the following basic requirements:

* Record the command entered by the examiner
* Record a timestamp for each command entered
* Record all commands, including duplicates, comments, and space-prefixed commands
* Avoid truncating or overwriting history files
* Avoid conflicts when using multiple terminal windows on the same system
* Include root and non-root command history

Important information, such as the command completion time, the working directory where the command was executed, and the return code, are not logged. The Bash history is also not a tamper-resistant system: the examiner can easily modify or delete the history.

Some shells, such as `zsh`, have additional history features that allow for the logging of elapsed time. Other proposed solutions to improve shell logging include the use of `PS1`, `PROMPT_COMMAND`, `trap` and `DEBUG`, and key bindings to modify a command before executing. Using `sudo logging`; `auditd logging`; or special scripts, such as [Bash-Preexec](https://github.com/rcaloras/bash-preexec), can also improve command line logging.

For basic shell command logging, the built-in shell history functionality can be configured to record command line activity. Add these commands to `~/.bashrc` or `/etc/profile`:

```bash
set -o history
shopt -s histappend
export HISTCONTROL=
export HISTIGNORE=
export HISTFILE=~/.bash_history
export HISTFILESIZE=-1
export HISTSIZE=-1
export HISTTIMEFORMAT="%F-%R "
```

These mean that history is enabled and in append mode (as opposed to overwriting with each new login). The two variables `HISTCONTROL`and `HISTIGNORE` control which commands are saved to the history file. A common default setting is to ignore duplicates and commands beginning with a space. To ensure complete logging of all commands, the `HISTCONTROL` and `HISTIGNORE` variables are explicitly set to null. The `HISTFILE` variable is explicitly set to ensure command history held in memory is saved when a shell exits. `HISTFILESIZE` and `HISTSIZE` are set to `-1` to ensure history is not truncated or overwritten. The `HISTTIMEFORMAT` variable enables timestamps to be written to the history file and allows for setting a time format.

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

## Drives

Attached drives will appear as block devices in the `/dev` directory when they’re detected by the kernel. Raw disk device files have a specific naming convention: `sd*` for SCSI and SATA, `hd*` for IDE, `md*` for RAID arrays, `nvme*n*` for NVME drives, and other names for less common or proprietary disk device drivers.

Individual partitions discovered by the kernel are represented by numbered raw devices (for example, `hda1`, `hda2`, `sda1`, `sda2`, and so forth).

When a new device is attached to (or removed from) a host, an interrupt notifies the kernel of a hardware change. The kernel informs the `udev` system, which creates appropriate devices with proper permissions, executes setup (or removal) scripts and programs, and sends messages to other daemons (via dbus, for example).

### Enumerating

To monitor received events:

```shell
udevadm monitor
```

Listing the associated files and paths for attached devices:

```shell
udevadm info /dev/sdx
```

### Special devices

Several other devices are useful to know: 

* The bit bucket, `/dev/null`, discards any data written to it. 
* A steady stream of zeros is provided when accessing `/dev/zero`. 
* The random number generator, `/dev/random`, provides a stream of random data when accessed. 
* Tape drives typically start with `/dev/st`.
* Access other external media via `/dev/cdrom` or `/dev/dvd` (these are often symbolic links to `/dev/sr*`). 
* In some cases, devices are accessed through the generic SCSI device driver interface `/dev/sg*`. 
* Other special pseudo devices include `/dev/loop*` and `/dev/mapper/*` devices.

## Files

### Enumerating

Recursively look for particular file types, and once you find the files get their hashes:

```shell
find . type f -exec sha256sum {} \; 2> /dev/null | grep -Ei '.asp|.js' | sort
```

Recursively list out folders and filders in their tree-branch relationship:

```shell
tree
```

Tree and show the users who own the files and directories:

```shell
tree -u
```

Stack with a `grep` to find a particular user:

```shell
tree -u | grep 'root'
```

Get information about a file:

```shell
stat file.txt
```

### Files and dates

Note: timestamps can be manipulated and can't be trusted during an IR.

Print the files and their corresponding timestamp:

```shell
find . -printf "%T+ %p\n"
```

Show all files created between two dates:

```shell
find -newerct "01 Jun 2021 18:30:00" ! -newerct "03 Jun 2021 19:00:00" -ls | sort
```

### Compare files

```shell
vimdiff file1.txt file2.txt
```

## Processes 

Track parent-child processes:

```shell
ps -aux --forest
```

Get an overview of every running process running from a non-standard path:

```shell
sudo ls -l /proc/[0-9]*/exe 2>/dev/null | awk '/ -> / && !/\/usr\/(lib(exec)?|s?bin)\// {print $9, $10, $11}' | sed 's,/proc/\([0-9]*\)/exe,\1,'
```

List every process:

```shell
sudo ls -l /proc/[0-9]*/exe 2>/dev/null | awk '/ -> / {print $NF}' | sort | tac
```

## Networks

Get a quick overview of network activity:

```shell
netstat -plunt
```

This alternative also helps re-visualise the originating command and user that a network connection belongs to:

```shell
sudo lsof -i
```

## Persistence

Persistence helps a malware persist on a machine after startup or any other mechanism that would stop a malware 
from running.

### Cron jobs

```shell
cat /etc/crontab
```

### Services

A list of services can be found in the `/etc/init.d` directory:

```shell
ls /etc/init.d/
```

### Bash

Check `.bashrc`:

```shell
cat ~/.bashrc
```

System wide settings:

```shell
/etc/bash.bashrc
/etc/profile
```

## Evidence of execution

Execution history of `sudo`:

```shell
cat /var/log/auth.log* |grep -i COMMAND|tail
```

Bash history:

```shell
cat ~/.bash_history
```

Check file access using `vim`:

```shell
cat ~/.viminfo
```

## Log files

`Syslog` contains messages that are recorded by the host about system activity:

```shell
cat /var/log/syslog* | head
```

`Auth` logs have information about users and authentication-related logs:

```shell
cat /var/log/auth.log* | head
```

`/var/log` contains logs for third-party applications, for example `Apache2` logs:

```shell
ls /var/log/apache2/
```

## Resources

* [You Don’t Know Jack About .bash_history - SANS DFIR Summit 2016](https://www.youtube.com/watch?v=wv1xqOV2RyE)
* [Linux Storage Stack Diagram](https://www.thomas-krenn.com/en/wiki/Linux_Storage_Stack_Diagram)