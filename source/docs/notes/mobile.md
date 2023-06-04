# Mobile data extraction

## iOS

### Extraction methods 

Three main acquisition methods are available for mobile devices: manual, logical, and physical, or file system for iOS. 

1. ***Manual data extraction***: This method is navigating the device as a normal user and taking screenshots of the found evidence. It is not a recommended acquisition method since it involves a high risk of human errors. This might affect the evidence state by accidental deletion of or changes to data. This is a very simple process and shows only what is seen on the device. Can be used only to validate outcomes in some cases. 
2. ***Logical data extraction***: Logical acquisition involves copying what the user has access to on their mobile, which means that data is extracted from backup. This method requires the device to be unlocked. It provides readable data, unlike some encrypted parts in the physical image. Recovering data from unallocated space is limited to data recovery from unallocated SQLite records. 
3. ***Physical data extraction***: The copying process in this method includes the device storage and the file system. The copying is done on the bits level acquiring all data. This includes deleted data and the ability to access the unallocated space. ***Physical acquisition is not useful for iPhone 5s and later*** due to the Secure Enclave hardware feature in Apple devices. It provides an additional layer of security by its isolation from the main processor. This security mechanism keeps the user data encrypted even if the OS is compromised. File system acquisition now is used for iOS devices, which requires a jailbroken device. Jailbreaking will change the original data on the device. It is not a reversible change.

## Android

### Partitions

Partition layout varies between device manufacturers and versions. Some of the common partitions found in most of the Android devices:

* Bootloader – This partition stores the phone’s bootloader program. This program takes care of initialising the low-level hardware when the phone boots.
* Boot – As the name suggests, this partition has the information and files required for the phone to boot. It contains the kernel and RAM disk. 
* Recovery – Recovery partition allows the device to boot into the recovery console through which activities such as phone updates and other maintenance operations are done. A minimal Android boot image is stored as a failsafe.
* Userdata – This partition contains the device's internal storage for application data, the bulk of user data, and standard communications.
* System – All the major components other than kernel and RAM disk: the Android framework, libraries, system binaries, and preinstalled applications. 
* Cache – This partition is used to store frequently accessed data and various other files, such as recovery logs and update packages downloaded over the network.
* Radio – Devices with phone capabilities have a baseband image stored in this partition.

### Extraction methods

1. ***Manual data extraction***: Standard interfaces such as touch controls, screen controllers, and keyboards are used to access the information stored on the device and to record input directly from the screen. There are no special tools necessary, and the technical difficulty is modest. Disadvantages of this method are that large amounts of data will be exhausted over time, there is a risk of data adjustments being made by mistake, and it does not restore data that has been erased. It will certainly be impractical if the hardware is destroyed.
2. ***Logical data extraction***: A standard interface between the workstation and the device is built using USB, Wi-Fi, or Bluetooth to send device data to the workstation. Technical complexity is minimal, but unintentional data changes may occur, and data access abstraction is high.
3. ***Physical data extraction or hex dumping***: With the device in diagnostic mode, its flash memory is downloaded. This can be done with conventional interfaces, and works with devices that have little damage. Data analysis and decoding might be difficult (JTAG requires training). Access to all partitions is not guaranteed.
4. ***Chip-off***: A binary image is extracted from the device's removed physical flash memory. This allows for traditional analysis, but can cause physical harm to the device. And examiners need training.
5. ***Micro read***: An electron microscope is used to examine logic gates on a physical level and the observations turned into readable, comprehensible data. This method is extremely resource-intensive and technically challenging.

## Logical data extraction

ADB usually runs with a non-privileged account. It will not provide access to internal application data. But on a rooted phone, ADB will run as root and provide access to internal application data and OS files and folders.

### Example using busybox

1. [Download](https://www.appsapk.com/busybox-app/) and install busybox apk:

```text
adb -d install BusyBox.apk
```

2. Check root access:

```text
su
# ls /data
```

3. Check the mounted partitions on the device:

```text
mount
```

Choose the partitions you wish to image and note their paths, for example for the data partition, something like `/dev/block/bootdevice/by-name/userdata`.

Set up connection between the workstation and the mobile device, forwarding port `8080`:
* 
Workstation:

```text
adb forward tcp:8080 tcp:8080
```

Mobile device:

```text
dd if=/dev/block/bootdevice/by-name/userdata | busybox nc -l -p 8080
```

Workstation:

```text
nc 127.0.0.1 8080 > android_data.dd
```

[Start analysis on the image disk](android.md), using sleuthkit tools or Autopsy.


## Resources

* [Android Debug Bridge (adb)](https://developer.android.com/tools/adb)
* [Kingo SuperUser](https://www.kingoapp.com/)
* [How to do a forensic analysis of Android 11 artifacts](https://www.semanticscholar.org/paper/How-to-do-a-forensic-analysis-of-Android-11-Delija-Sudec/e9e48ca6c3a5a21c6233c30f220db1a82a89441e)