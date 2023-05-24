# iOS forensics

The first step in a forensic examination of an iOS device is identifying the device model. The model of an iOS device can be used to help the examiner develop an understanding of the underlying components and capabilities of the device, which can be used to drive the methods for acquisition and examination. Legacy iOS devices should not be disregarded, because they may surface as part of an investigation. Examiners must be aware of all iOS devices, as old devices are sometimes still in use and may be tied to a criminal investigation.

## Acquisition

There are several ways to acquire data from an iOS device: logical, filesystem, and physical acquisition techniques, including techniques of acquiring jailbroken devices and methods to bypass passcodes. 

Physical acquisition is the preferred acquisition method as it recovers as close as possible a bit-by-bit copy of the data from the device; but, it is not possible to perform physical acquisition on all iOS devices.

Backup files may exist or be the only method to extract data from the device. iOS device backups contain essential information that may be the only source of evidence. Information stored in iOS backups includes photos, videos, contacts, email, call logs, user accounts and passwords, applications, device settings, and more.

## Common iOS artifacts