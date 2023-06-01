# Mobile forensics

The term "mobile devices" includes mobile phones, smartphones, tablets, and GPS units to wearables and PDAs. These small-sized machines amass huge quantities of data on a daily basis, which can be extracted to facilitate an investigation.

* Incoming, outgoing, missed call history
* Phonebook or contact lists
* SMS text, application based, and multimedia messaging content
* Pictures, videos, and audio files and sometimes voicemail messages
* Internet browsing history, content, cookies, search history, analytics information
* To-do lists, notes, calendar entries, ringtones
* Documents, spreadsheets, presentation files and other user-created data
* Passwords, passcodes, swipe codes, user account credentials
* Historical geolocation data, cell phone tower related location data, Wi-Fi connection information
* User dictionary content
* Data from various installed apps
* System files, usage logs, error messages
* Deleted data from all of the above

It is hard to be in control of data on mobile devices because the data is mobile as well. Once communications or files are sent from a smartphone, control is lost. Although there are different devices having the capability to store considerable amounts of data, the data in itself may physically be in another location. 

## iOS

The first step in a forensic examination of an iOS device is identifying the device model. The model of an iOS device can be used to help the examiner develop an understanding of the underlying components and capabilities of the device, which can be used to drive the methods for acquisition and examination. Legacy iOS devices should not be disregarded, because they may surface as part of an investigation. Examiners must be aware of all iOS devices, as old devices are sometimes still in use and may be tied to a criminal investigation.

There are several ways to acquire data from an iOS device: logical, filesystem, and physical acquisition techniques, including techniques of acquiring jailbroken devices and methods to bypass passcodes. 

Physical acquisition is the preferred acquisition method as it recovers as close as possible a bit-by-bit copy of the data from the device; but, it is not possible to perform physical acquisition on all iOS devices.

Backup files may exist or be the only method to extract data from the device. iOS device backups contain essential information that may be the only source of evidence. 

## Android

Unlike iOS, several variants of Android exist as many devices run the Android operating system and each may have different file systems and unique features. The fact that Android is open and customizable also changes the digital forensics of it. Expect the unexpected when handling an Android device.

A proper forensic workstation setup is required prior to conducting investigations on an Android device. Using open source methods to acquire and analyze Android devices requires the installation of specific software on a forensic workstation. If the method of forensic acquisition requires the Android device to be unlocked, determine the best method by which to gain access to the device. Depending on the forensic acquisition method and scope of the investigation, rooting the device can provide complete access to the files present on the device.

Once the device is accessible, an examiner can extract the data using manual, logical, or physical data extraction techniques. Logical techniques extract the data by interacting with the device using tools such as ADB. Physical techniques access a larger set of data; they are complex and require expertise.

Recovery of the deleted data on Android devices depends on various factors, which heavily rely on access to the data residing in the internal memory and SD card. While the recovery of deleted items from external storage, such as an SD card, is easy, the recovery of deleted items from the internal memory takes considerable effort. SQLite file-parsing and file-carving techniques are two methods that are used to recover deleted data extracted from an Android device.

