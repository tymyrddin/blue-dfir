# Image acquisition

Maximizing the amount of data extracted from a storage medium, minimizing the disturbance to the storage device and medium, preserving the collected evidence, and documenting the process (including errors).

Assumptions:

* The storage device is physically attached to the forensics examiner’s acquisition workstation.
* The storage device has been positively identified.
* The appropriate write-blocking mitigation is in place to prevent modification of the subject drive.
* Disk capacity planning has been performed to ensure disk space is not an issue.

## dd tools

### Standard dd

To copy a disk block device to a file:

    # dd if=/dev/sde of=image.raw

with protection from unreadable blocks by skipping (`noerror`) and padding them with zeros (`sync`):

    # dd if=/dev/sde of=image.raw conv=noerror,sync

### dcfldd

To image a disk, ensuring blocks containing unreadable sectors are padded:

    # dcfldd if=/dev/sde of=image.raw conv=noerror,sync errlog=error.log

### dc3dd

To image a disk, adding the `conv=noerror,sync` flag is not needed because it is built in:

    # dc3dd if=/dev/sde of=image.raw log=error.log

Traditional `dd` has no capability for hashing, logging to a file, or other features handy for forensic acquisition. Both `dcfldd` and `dc3dd` have additional features for cryptographic hashing, image splitting, and piping to external programs.

## Forensic formats

Several imaging formats, like FTK and EnCase for example, were specifically designed with forensics in mind. These are commercial proprietary formats and have been reverse engineered to allow development of open source–compatible tools.

### ewfacquire

`libewf` is a library to access the Expert Witness Compression Format (EWF). Its `ewfacquire` tool creates acquisition files that enable interoperability with EnCase, FTK, and Sleuth Kit. The tool can also convert raw images into other formats.

To acquire an attached disk device (a MacBook Air connected to the examiner workstation in Target Disk Mode with a Thunderbolt-to-FireWire adapter):

```text
# ewfacquire -c best -t /exam/macbookair /dev/sdf
```

### ftkimager

The ftkimager tool can take input from a raw device, a file, or `stdin`. It outputs to an FTK SMART format, an EnCase EWF format, or `stdout`. A number of other features are supported, including the addition of case metadata into the saved formats, compression, output file splitting ("image fragments"), hashing, and encrypted images.

Example:

```text
# ftkimager /dev/sdf --s01 --description "serial number and model string" sandisk
```

The ftkimager creates a log file with basic metadata and any additional information that was added using flags on the command line.

### SquashFS forensic evidence container

Configure [sfsimage](testlab:docs/dfir/squashfs) to use `dc3dd` as the imaging tool by editing the `DD` variable in the beginning of the shell script:

    DD="dc3dd if=$DDIN log=errorlog.txt hlog=hashlog.txt hash=md5"

