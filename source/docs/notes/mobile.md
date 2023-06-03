# Mobile data extraction

## iOS

### Extraction methods 

Three main acquisition methods are available for mobile devices: manual, logical, and physical, or file system for iOS. 

1. ***Manual data extraction***: This method is navigating the device as a normal user and taking screenshots of the found evidence. It is not a recommended acquisition method since it involves a high risk of human errors. This might affect the evidence state by accidental deletion of or changes to data. This is a very simple process and shows only what is seen on the device. Can be used only to validate outcomes in some cases. 
2. ***Logical data extraction***: Logical acquisition involves copying what the user has access to on their mobile, which means that data is extracted from backup. This method requires the device to be unlocked. It provides readable data, unlike some encrypted parts in the physical image. Recovering data from unallocated space is limited to data recovery from unallocated SQLite records. 
3. ***Physical data extraction***: The copying process in this method includes the device storage and the file system. The copying is done on the bits level acquiring all data. This includes deleted data and the ability to access the unallocated space. ***Physical acquisition is not useful for iPhone 5s and later*** due to the Secure Enclave hardware feature in Apple devices. It provides an additional layer of security by its isolation from the main processor. This security mechanism keeps the user data encrypted even if the OS is compromised. File system acquisition now is used for iOS devices, which requires a jailbroken device. Jailbreaking will change the original data on the device. It is not a reversible change.

## Android

### Extraction methods

1. ***Manual data extraction***: Standard interfaces such as touch controls, screen controllers, and keyboards are used to access the information stored on the device and to record input directly from the screen. There are no special tools necessary, and the technical difficulty is modest. Disadvantages of this method are that large amounts of data will be exhausted over time, there is a risk of data adjustments being made by mistake, and it does not restore data that has been erased. It will certainly be impractical if the hardware is destroyed.
2. ***Logical data extraction***: A standard interface between the workstation and the device is built using USB, Wi-Fi, or Bluetooth to send device data to the workstation. Technical complexity is minimal, but unintentional data changes may occur, and data access abstraction is high.
3. ***Physical data extraction or hex dumping***: With the device in diagnostic mode, its flash memory is downloaded. This can be done with conventional interfaces, and works with devices that have little damage. Data analysis and decoding might be difficult (JTAG requires training). Access to all partitions is not guaranteed.
4. ***Chip-off***: A binary image is extracted from the device's removed physical flash memory. This allows for traditional analysis, but can cause physical harm to the device. And examiners need training.
5. ***Micro read***: An electron microscope is used to examine logic gates on a physical level and the observations turned into readable, comprehensible data. This method is extremely resource-intensive and technically challenging.




