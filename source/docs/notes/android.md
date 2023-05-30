# Android forensics

Unlike iOS, several variants of Android exist as many devices run the Android operating system and each may have different file systems and unique features. The fact that Android is open and customizable also changes the digital forensics of it. Expect the unexpected when handling an Android device.

A proper forensic workstation setup is required prior to conducting investigations on an Android device. Using open source methods to acquire and analyze Android devices requires the installation of specific software on a forensic workstation.

## Pre-Data extraction

If the method of forensic acquisition requires the Android device to be unlocked, determine the best method by which to gain access to the device. Depending on the forensic acquisition method and scope of the investigation, rooting the device should provide complete access to the files present on the device.

## Data extraction techniques

Once the device is accessible, an examiner can extract the data using manual, logical, or physical data extraction techniques. Logical techniques extract the data by interacting with the device using tools such as ADB. Physical techniques, on the other hand, access a larger set of data; they are complex and require a great deal of expertise.

## Data analysis and recovery

Recovery of the deleted data on Android devices depends on various factors, which heavily rely on access to the data residing in the internal memory and SD card. While the recovery of deleted items from external storage, such as an SD card, is easy, the recovery of deleted items from the internal memory takes considerable effort. SQLite file-parsing and file-carving techniques are two methods that are used to recover deleted data extracted from an
Android device.
