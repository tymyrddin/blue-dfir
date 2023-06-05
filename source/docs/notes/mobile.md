# Mobile data extraction

## Extraction methods

1. ***Manual data extraction***: This method is navigating the device as a normal user, using touch controls, screen controllers, and keyboards to access the information stored on the device and to record input directly from the screen. There are no special tools necessary, and the technical difficulty is modest. Disadvantages of this method are that large amounts of data will be exhausted over time, there is a risk of data adjustments being made by mistake, and it does not restore data that has been erased. It will certainly be impractical if the hardware is destroyed. It ***can*** be useful for validating outcomes. 
2. ***Logical data extraction***: A standard interface between the workstation and the device is built using USB, Wi-Fi, or Bluetooth to send device data to the workstation. Technical complexity is minimal, but unintentional data changes may occur, and data access abstraction is high. For iOS, logical acquisition involves copying what the user has access to on their mobile, which means that data is extracted from backup. This method requires the device to be unlocked. It provides readable data, unlike some encrypted parts in the physical image. Recovering data from unallocated space is limited to data recovery from unallocated SQLite records. 
3. ***Physical data extraction or hex dumping***: With the device in diagnostic mode, its flash memory is downloaded. This can be done with conventional interfaces, and works with devices that have little damage. Data analysis and decoding might be difficult (JTAG requires training). Access to all partitions is not guaranteed. For iOS, the copying process in this method includes the device storage and the file system. The copying is done on the bits level acquiring all data. This includes deleted data and the ability to access the unallocated space. ***Physical acquisition is not useful for iPhone 5s and later*** due to the Secure Enclave hardware feature in Apple devices. It provides an additional layer of security by its isolation from the main processor. This security mechanism keeps the user data encrypted even if the OS is compromised. File system acquisition now is used for iOS devices, which requires a jailbroken device. Jailbreaking will change the original data on the device. It is not a reversible change.
4. ***Chip-off***: A binary image is extracted from the device's removed physical flash memory. This allows for traditional analysis, but can cause physical harm to the device. And examiners need training.
5. ***Micro read***: An electron microscope is used to examine logic gates on a physical level and the observations turned into readable, comprehensible data. This method is extremely resource-intensive and technically challenging.

## Android partitions

Partition layout varies between device manufacturers and versions. Some of the common partitions found in most of the Android devices:

* Bootloader – This partition stores the phone’s bootloader program. This program takes care of initialising the low-level hardware when the phone boots.
* Boot – As the name suggests, this partition has the information and files required for the phone to boot. It contains the kernel and RAM disk. 
* Recovery – Recovery partition allows the device to boot into the recovery console through which activities such as phone updates and other maintenance operations are done. A minimal Android boot image is stored as a failsafe.
* Userdata – This partition contains the device's internal storage for application data, the bulk of user data, and standard communications.
* System – All the major components other than kernel and RAM disk: the Android framework, libraries, system binaries, and preinstalled applications. 
* Cache – This partition is used to store frequently accessed data and various other files, such as recovery logs and update packages downloaded over the network.
* Radio – Devices with phone capabilities have a baseband image stored in this partition.

## Android logical data extraction

* Enable ***USB debugging***.
* Enable the ***Stay awake*** setting.
* Increase ***screen timeout***.
* The device needs to be isolated from the network to make sure that remote wipe options do not work on the device.

### Connecting device to a workstation

1. Identify the device cable: The physical USB interface of an Android device might change from manufacturer to manufacturer and from device to device: Mini - A USB, Micro - B USB, Co-axial (Nokia), D Sub-miniature (Samsung and LG devices)
2. Install the device drivers if the device is not recognised. Because Android is allowed to be modified and customized by the manufacturers, there is no single generic driver that works for all Android devices. Each manufacturer writes its own proprietary drivers and distributes them over the internet. Some Android forensic toolkits come with some generic drivers or a set of the most widely used drivers.
3. Access: connect the unlocked Android device to the computer directly using the USB cable. The Android device will appear as a new drive and you will be able to access the files on the external storage. Some older Android devices may not be accessible unless the ***Turn on USB storage*** option is enabled. In some Android phones (especially with HTC), the device may expose more than one functionality when connected with a USB cable. Mount it as a disk drive to access the SD card.
4. In order to give the best chance of accessing the evidence at a later date, if the device is unlocked, then turn on USB debugging if possible. 

### USB debugging

The primary function of this option is to enable communication between the Android device and a workstation on which the Android SDK is installed. On a Samsung phone, access this under ***Settings -> Developer Options***. Other Android phones may have different environments and configuration features. You may have to force the ***Developer Options*** option by accessing build mode.

In Android version 4.2 and above, the developer options screen is hidden. In order to turn on USB debugging go into the settings menu, and go to the ***About phone*** or ***About tablet***, and a field showing the Android ***build number*** appears. For Samsung, tap ***About phone***, then ***Software information*** to find ***build number***. If the android build number is tapped seven times, developer mode is enabled and the "Developer options" menu will appear above the "About phone" menu. From there, USB debugging can be turned on. You may also be able to adjust the Display Screen Timeout feature to extend the length of time before Auto-Lock is enabled.

Starting from Android 4.2.2, Google has introduced the Secure USB debugging option. It only allows hosts that are explicitly authorised by the user to connect to the device using ADB. 

When the ***USB debugging*** option is selected, the device will run the adb daemon (`adbd`) in the background and will continuously look for a USB connection. The daemon will usually run under a non-privileged shell user account and thus will not provide access to the complete data. However, on rooted phones, `adbd` will run under the `root` account and thus provide access to all the data. It is not recommended to ***root a device*** to gain full access unless all other forensic methods fail. Should you choose to root an Android device, the methods must be well-documented and tested prior to attempting it on real evidence.

### Android Debug Bridge

The Android Debug Bridge (ADB) is a programming tool for debugging Android-based devices. The daemon on the Android device communicates with the server on the host PC through USB or TCP, which communicates with the end-client users via TCP.

To get a list of all the devices connected to the forensic workstation:

    adb devices

To kill the local adb service:

    adb kill-server

To access the shell on an Android device and interact with the device:

```text
nina@tardis:~$ adb shell
a50:/ $ 
```

Many linux commands can be used. See cheatsheets.

### Stay awake

Enable the ***Stay awake*** setting: If the Stay awake option is selected and the device is connected for charging, then the device never locks. Again, if the device locks, the acquisition can be halted.

### Increase screen timeout

Screen timeout is the time for which the device will be effectively active once it is unlocked. The location to access this setting varies
depending upon the model of the device. On a Samsung Galaxy, you can set it by navigating to ***Settings -> Display -> ScreenTimeout***.

## Screen lock bypassing techniques Android

There are three types of screen lock mechanisms offered by Android:

* Android was the first smartphone to introduce a ***pattern lock***, a pattern or design on the phone and the same pattern must be drawn to unlock the device. 
* ***PIN code*** is the most common lock option and is a 4-digit number that needs to be entered to unlock the device. 
* Unlike the PIN, which takes four digits, the alphanumeric ***passcode*** includes letters, as well as digits.

### Delete gesture.key

This method works when the device is rooted. This method may not be successful on unrooted devices. Also note, deleting the `gesture.key` file will remove the pattern lock on the device, but this will permanently change the device, as the pattern lock is gone. 

1. Connect the device to the forensic workstation using a USB cable.
2. Open the command prompt and execute:

```text
adb shell
cd /data/system
rm gesture.key
```

3. Reboot the device.
4. Unlock device with whatever pattern.

### Update the settings.db file

1. Connect your device with the computer via USB cable.
2. Open command prompt and execute:

```text
adb shell
cd /data/data/com.android.providers.settings/databases
sqlite3 settings.db
update system set value=0 where name='lock_pattern_autolock';
update system set value=0 where name='lockscreen.lockedoutpermanently';
.quit
```

3. Reboot the device.
4. Unlock device with whatever pattern.

## Imaging using busybox

ADB usually runs with a non-privileged account. It will not provide access to internal application data. On a rooted phone, ADB will run as root and provide access to internal application data and OS files and folders.

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